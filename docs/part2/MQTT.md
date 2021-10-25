# Sending Data Using MQTT

## Lab Objectives

In this lab you will learn how to add MQTT messaging to an application.  You will learn:

- How to connect to a MQTT broker using unsecured connection
- How to use MQTT to connect to a cloud-based MQTT broker

## Introduction

In the previous lab you built the stand alone sensor application.  Now we want to make it an Internet of Things application by adding in MQTT to send the data to a cloud-based MQTT broker.

Once the data reaches a suitable MQTT broker, it can then be more easily routed to other services hosted in the Cloud (such as Node-RED, for visualisation and control, and Jupiter, for data analysis).

The software you are building will not generate much data and there are a number of public MQTT brokers you could connect your ESP8266 to, such as the public MQTT brokers hosted by mosquitto and hiveMQ. 

We will start by using an unsecured MQTT connection, then in the next section we will look at how you we can secure the connection.  

The Mosquitto Project runs a test server at test.mosquitto.org where you can test your client in a variety of ways: plain MQTT, MQTT over TLS, and MQTT over TLS (with client certificate), 
MQTT over WebSockets and MQTT over WebSockets with TLS.

HiveMQ runs a test server at broker.hivemq.com (see https://www.hivemq.com/public-mqtt-broker/) but does not enable testing secure connections.

Alternatively, you may find other suitable MQTT brokers, including MQTT brokers incorporated in IoT Cloud platforms providing different services all-in-one, which may be convenient
as you would not need to setup multiple accounts and use different dashboards for different services. 

### Step 1 - Enhancing the application to send data to the an IoT platform

In the Arduino IDE you need to add the MQTT code, but before adding the MQTT code you need to install the library.  In the library manager (*Sketch* -> *Include Library* -> *Manage Libraries...*) search for and install the PubSubClient.  Then add the include to the top of the application, below the existing include files

```#include <PubSubClient.h>```

Now add some #define statements to contain that the MQTT code will use.  Add these under the comment **UPDATE CONFIGURATION TO MATCH YOUR ENVIRONMENT**:

```C++
// --------------------------------------------------------------------------------------------
//        UPDATE CONFIGURATION TO MATCH YOUR ENVIRONMENT
// --------------------------------------------------------------------------------------------

// MQTT connection details
#define MQTT_HOST "<Replace this with the address of your MQTT broker>"
#define MQTT_PORT 1883
#define MQTT_DEVICEID "<replace this with a unique identifier for your device, e.g. d:hwu:esp8266:< **device id** >"
#define MQTT_USER "" // no need for authentication, for now
#define MQTT_TOKEN "" // no need for authentication, for now
#define MQTT_TOPIC "< **device id** >/evt/status/fmt/json"
#define MQTT_TOPIC_DISPLAY "< **device id** >/cmd/display/fmt/json"
```

You need to change the values to match your configuration:

The value you choose for MQTT_DEVICEID should uniquely identify your ESP8266 with the MQTT broker. 
This is especially important if you are using a public one, shared with other users.
Some MQTT brokers (especially if they are part of IoT platforms, such as IBM Cloud) will ask you to use specific formats. Others will let you choose your own format.
Either way, the reccomended format should follow a hierarchical style, i.e. identifying your organisation, then the device type, and finally the specific device, e.g.
you could use "d:hwu.esp8266.< ** your student ID ** >", replacing every instance of < **device id** > with your student ID, since each student will be using a single ESP8266.

After the configuration block and under the pixel and dht variable declarations you need to add the the following:

```C++
// MQTT objects
void callback(char* topic, byte* payload, unsigned int length);
WiFiClient wifiClient;
PubSubClient mqtt(MQTT_HOST, MQTT_PORT, callback, wifiClient);
```

Above the setup() function add the implementation of the callback function.  This is called whenever a MQTT message is sent to the device.  For now it just prints a message to the serial console:

```C++
void callback(char* topic, byte* payload, unsigned int length) {
  // handle message arrived
  Serial.print("Message arrived [");
  Serial.print(topic);
  Serial.print("] : ");
  
  payload[length] = 0; // ensure valid content is zero terminated so can treat as c-string
  Serial.println((char *)payload);
}
```

at the end of the setup() function add the following code to connect the MQTT client to the IoT Platform:

```C++
  // Connect to MQTT broker
  if (mqtt.connect(MQTT_DEVICEID, MQTT_USER, MQTT_TOKEN)) {
    Serial.println("MQTT Connected");
    mqtt.subscribe(MQTT_TOPIC_DISPLAY);

  } else {
    Serial.println("MQTT Failed to connect!");
    ESP.reset();
  }
```

at the top of the loop() function add the following code to verify the mqtt connection is still valid and call the mqtt.loop() function to process any outstanding messages:

```C++
  mqtt.loop();
  while (!mqtt.connected()) {
    Serial.print("Attempting MQTT connection...");
    // Attempt to connect
    if (mqtt.connect(MQTT_DEVICEID, MQTT_USER, MQTT_TOKEN)) {
      Serial.println("MQTT Connected");
      mqtt.subscribe(MQTT_TOPIC_DISPLAY);
      mqtt.loop();
    } else {
      Serial.println("MQTT Failed to connect!");
      delay(5000);
    }
  }
```

Lastly add the code to send the data to the IoT Platform.  We already have the data formatted as a JSON string, so we can now add the following code after it is printed to the console in the **loop()** function:

```C++
    Serial.println(msg);
    if (!mqtt.publish(MQTT_TOPIC, msg)) {
      Serial.println("MQTT Publish failed");
    }
```

Finally, replace the 10 second ```delay(10000)``` to call the mqtt **loop()** function, so the program processes incoming messages:

```C++
  // Pause - but keep polling MQTT for incoming messages
  for (int i = 0; i < 10; i++) {
    mqtt.loop();
    delay(1000);
  }
```

### Step 3 - Run the application

Compile and upload the code to your ESP8266 and you should see the ```WiFi Connected```, followed by ```Attempting MQTT connection...MQTT Connected```. Every 10 second interval you see the DHT sensor data printed on the console.  
The ESP8266 should also be publishing MQTT messages to the MQTT Broker.
To verify this, you should use another MQTT client. This could be a mosquitto client installed on your laptop, but also a web-based client, conveniently offered by both hiveMQ and mosquitto.
You'd need to subscribe to the topic published by your device. After that, you should see the status event messages with the live data appearing every 10 seconds.
This is an opportunity to practice using wildcards when registering to a topic in MQTT. Think what'd you need to do, to view the updates from multiple devices.
You can verify this, by asking other students to post the status updates from their devices to the same MQTT broker you use. 
 
### Step 4 - How it works

When connecting a MQTT broker, there are some requirements on some parameters used when connecting.  
The documentation of the broker you have chosen (e.g. hiveMQ or mosquitto) will provides full details.

1. The #define statements construct the required parameters:
   - host : < this is the Internet address of your MQTT broker, e.g. the address of the public hiveMQ broker is "broker.hivemq.com" >
   - device ID : d:< **org id** >:< **device type** >:< **device id** >
   - topic to publish data : < **device id** >/evt/< **event id** >/fmt/<  **format string** >
   - topic to receive commands : < **device id** >/cmd/< **command id** >/fmt/< **format string** >
2. When you initialise the PubSubClient you need to pass in the hostname, the port (1883 for unsecured connections), a callback function and a network connection.  The callback function is called whenever incoming messages are received.
3. Call **connect()** to connect with the platform, passing in the device ID, a user, which is always the value *use-token-auth* and the token you chose when registering the device.
4. The **subscribe()** function registers the connection to receive messages published on the given topic.
5. The **loop()** method must be regularly called to keep the connection alive and get incoming messages.
6. The **publish()** function sends data on the provided topic

    !!!Note
        On some MQTT Client libraries this function only queues the message for sending, it is actually sent in the **loop()** function

7. You can verify the connection status with the **connected()** function.

### Solution code

The complete ESP8266 application is shown below (you will need to change the configuration section to match your environment):

```C++
#include <ESP8266WiFi.h>
#include <Adafruit_NeoPixel.h>
#include <DHT.h>
#include <ArduinoJson.h>
#include <PubSubClient.h>

// --------------------------------------------------------------------------------------------
//        UPDATE CONFIGURATION TO MATCH YOUR ENVIRONMENT
// --------------------------------------------------------------------------------------------

// Watson IoT connection details
#define MQTT_HOST "<Replace this with the address of your MQTT broker>"
#define MQTT_PORT 1883
#define MQTT_DEVICEID "<replace this with a unique identifier for your device, e.g. d:hwu:esp8266:< **device id** >"
#define MQTT_USER "" // no need for authentication, for now
#define MQTT_TOKEN "" // no need for authentication, for now
#define MQTT_TOPIC "< **device id** >/evt/status/fmt/json"
#define MQTT_TOPIC_DISPLAY "< **device id** >/cmd/display/fmt/json"

// Add GPIO pins used to connect devices
#define RGB_PIN 5 // GPIO pin the data line of RGB LED is connected to
#define DHT_PIN 4 // GPIO pin the data line of the DHT sensor is connected to

// Specify DHT11 (Blue) or DHT22 (White) sensor
#define DHTTYPE DHT11
#define NEOPIXEL_TYPE NEO_RGB + NEO_KHZ800

// Temperatures to set LED by (assume temp in C)
#define ALARM_COLD 0.0
#define ALARM_HOT 30.0
#define WARN_COLD 10.0
#define WARN_HOT 25.0


// Add WiFi connection information
char ssid[] = "SSID";     //  your network SSID (name)
char pass[] = "WiFi_password";  // your network password


// --------------------------------------------------------------------------------------------
//        SHOULD NOT NEED TO CHANGE ANYTHING BELOW THIS LINE
// --------------------------------------------------------------------------------------------
Adafruit_NeoPixel pixel = Adafruit_NeoPixel(1, RGB_PIN, NEOPIXEL_TYPE);
DHT dht(DHT_PIN, DHTTYPE);


// MQTT objects
void callback(char* topic, byte* payload, unsigned int length);
WiFiClient wifiClient;
PubSubClient mqtt(MQTT_HOST, MQTT_PORT, callback, wifiClient);

// variables to hold data
StaticJsonDocument<100> jsonDoc;
JsonObject payload = jsonDoc.to<JsonObject>();
JsonObject status = payload.createNestedObject("d");
static char msg[50];

float h = 0.0;
float t = 0.0;

unsigned char r = 0;
unsigned char g = 0;
unsigned char b = 0;

void callback(char* topic, byte* payload, unsigned int length) {
  // handle message arrived
  Serial.print("Message arrived [");
  Serial.print(topic);
  Serial.print("] : ");
  
  payload[length] = 0; // ensure valid content is zero terminated so can treat as c-string
  Serial.println((char *)payload);
}

void setup() {
 // Start serial console
  Serial.begin(115200);
  Serial.setTimeout(2000);
  while (!Serial) { }
  Serial.println();
  Serial.println("ESP8266 Sensor Application");

  // Start WiFi connection
  WiFi.mode(WIFI_STA);
  WiFi.begin(ssid, pass);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("");
  Serial.println("WiFi Connected");

  // Start connected devices
  dht.begin();
  pixel.begin();

  // Connect to MQTT broker
  if (mqtt.connect(MQTT_DEVICEID, MQTT_USER, MQTT_TOKEN)) {
    Serial.println("MQTT Connected");
    mqtt.subscribe(MQTT_TOPIC_DISPLAY);

  } else {
    Serial.println("MQTT Failed to connect!");
    ESP.reset();
  }
}

void loop() {
  mqtt.loop();
  while (!mqtt.connected()) {
    Serial.print("Attempting MQTT connection...");
    // Attempt to connect
    if (mqtt.connect(MQTT_DEVICEID, MQTT_USER, MQTT_TOKEN)) {
      Serial.println("MQTT Connected");
      mqtt.subscribe(MQTT_TOPIC_DISPLAY);
      mqtt.loop();
    } else {
      Serial.println("MQTT Failed to connect!");
      delay(5000);
    }
  }
  h = dht.readHumidity();
  t = dht.readTemperature(); // uncomment this line for centigrade
  // t = dht.readTemperature(true); // uncomment this line for Fahrenheit

  // Check if any reads failed and exit early (to try again).
  if (isnan(h) || isnan(t)) {
    Serial.println("Failed to read from DHT sensor!");
  } else {
    // Set RGB LED Colour based on temp
    b = (t < ALARM_COLD) ? 255 : ((t < WARN_COLD) ? 150 : 0);
    r = (t >= ALARM_HOT) ? 255 : ((t > WARN_HOT) ? 150 : 0);
    g = (t > ALARM_COLD) ? ((t <= WARN_HOT) ? 255 : ((t < ALARM_HOT) ? 150 : 0)) : 0;
    pixel.setPixelColor(0, r, g, b);
    pixel.show();

    // Send data to Watson IoT Platform
    status["temp"] = t;
    status["humidity"] = h;
    serializeJson(jsonDoc, msg, 50);
    Serial.println(msg);
    if (!mqtt.publish(MQTT_TOPIC, msg)) {
      Serial.println("MQTT Publish failed");
    }
  }

  // Pause - but keep polling MQTT for incoming messages
  for (int i = 0; i < 10; i++) {
    mqtt.loop();
    delay(1000);
  }
}
```

---

[Click to return to the Part 2 homepage.](https://care-group.github.io/ESP866-IoT-Workshop/docs/part2/)