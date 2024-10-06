# Receive Device Environmental Sensor Data in Node-RED

## Lab Objectives

In this lab you will build a flow that receives Device environmental temperature and humidity sensor data. You will learn:

- How to create a new Node-RED flow and configure MQTT Nodes
- How to output the Device environmental temperature and humidity data.
- How to work with JSON data and observe the sensor results in the Debug sidebar.

## Introduction

In just a few nodes, Node-RED can receive the data that was transmitted from the device over MQTT. This simple exercise will be the foundation for the next several sections that plot the data in a dashboard, trigger Real Time threshold alerts, store the data in Cloud Storage and allow for data analytics and anomaly detection.

## MQTT Application Connections

In Part 2 you connected the ESP32 application to a MQTT broker. In this section you will use a MQTT client in your Node-RED application.


### Custom Certificates

If you have a custom server certificate configured on your MQTT broker, then you must use the SNI (Server Name Indication) extension to the TLS protocol to specify the server name in the connection parameters or Node-RED will not use your custom certificate.

## Create the Node-RED Flow to Receive Device Events

---
**IMPORTANT**

The following step 1 helps you to configure a MQTT input node over a secure connection with your MQTT broker. If you are using non-secure connections, you will need to change the port (to 1883) and you will not need to fill authorisation-related fields.

---

### Step 1 - Configure an MQTT Input in Node-RED

- From the Input category of the left Node-RED palette, find and select an **mqtt in node** and drag it onto your Node-RED flow.
- Double-click on the MQTT node. An **Edit mqtt in node** sidebar will open.
- Click the pencil icon next to the Server property to configure the MQTT server properties - this will open the **mqtt-broker node** sidebar:
  - Enter the MQTT server name in the Server field
  - Enter 8883 as the Port 
  - Enter the Client ID (a:*orgId*:*appId*) - the appID can be any unique string
  - Enable secure (SSL/TLS) connection then select the pencil next to the TLS configuration to open the **Edit tls-config node** sidebar:
    - Upload the rootCA_certificate.pem file you created in part 2 that is enabled on your server as the CA Certificate
    - Enable Verify server certificate
    - Enter the Server Name (*orgId*.messaging.internetofthings.ibmcloud.com), which sets the SNI extension on the TLS connection
    - Select Update to close the **Edit tls-config node** ![TLS Config](screenshots/TLSconfig.png)
  - You have finished on the connection tab ![broker connection](screenshots/mqttBrokerConnection.png)
  - Switch to the Security tab and enter the Username and Password to connect to your MQTT broker
 ![mqtt Broker Security](screenshots/mqttBrokerSecurity.png)
  - Press the Update button to return to the **Edit mqtt in node** sidepanel
- Set the topic field to receive the data published by your device.  You could sue wildcard **+**, to select all events from all devices and all events. 
  The topic requires that the event contains JSON data
- Select the output as **a parsed JSON object** ![MQTT in node config](screenshots/mqttInNodeConfig.png)
- Press Done to close the config sidepanel

### Step 2 - Extract the Temperature from the JSON Object

- Recall that the environmental sensor data was transmitted in a JSON object

 ```{ "d": {"temp":X, "humidity":Y }}```

- Node-RED passes data from node to node in a *msg.payload* JSON object.
- The **Change** node can be used to extract a particular value so that it can be directly output or manipulated (for instance in a Dashboard chart which we will take advantage of in the next section).
- From the Function category of the left Node-RED palette, select a **Change** node and drag it onto your Node-RED flow
- Double-click on the Change node. An **Edit change node** sidebar will open
- Configure the "to" AZ dropdown to msg. and set it to *payload.d.temp*
- Click on the red **Done** button
- Wire the node to the MQTT in node by clicking and dragging the connector on the right of the MQTT in node to the connector on the left of the change node
 ![Receive DHT Data](screenshots/ESP32-ReceiveDHTdata-Changenode.png)

## Step 3 - Node-RED Debug Nodes

- Debug nodes can be used to print out JSON object values and help you validate your program.
- From the Output category of the left Node-RED palette, drag two **debug nodes** onto your Node-RED flow (7).
- Double-click on one of them. An **Edit debug node** sidebar will open.
- Configure the Output to print the *complete msg object* (8).
- Click on the red **Done** button.
- Wire the 2 nodes as shown
 ![Receive DHT Data](screenshots/ESP32-ReceiveDHTdata-Debugnode.png)

### Step 4 - Wire the Node-RED nodes together

- Click on the red **Deploy** button in the upper right corner.
  - The **mqtt in** node should show status **Connected**
  - Observe the DHT sensor data in the **debug** tab of the Node-RED right sidebar. You can expand the twisties to expose the JSON object information. Hover over a debug message in the right sidebar and the node that generated the message will be outlined in orange.
  ![Receive DHT Data](screenshots/ESP32-ReceiveDHTdata-Deploy.png)

---

[Click to return to the Part 3 homepage.](https://care-group.github.io/ESP866-IoT-Workshop/docs/part3/)
