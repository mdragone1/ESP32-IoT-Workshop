# Connecting the ESP32 to a WiFi network

## Lab Objectives

This lab will show you how to connection your ESP32 to a local WiFi network. This Lab will also introduce the Serial Monitor, which allows you to see output from a running application. By the end of this lab you should:

- Be able to add a WiFi connection to a sketch
- Be able to add a Serial connection and generate output to the serial connection.
- Understand the function calls needed to start up the WiFi and Serial connections then how to use the connections and where to find documentation about the functions available.

## Introduction

In the First App practical you verified you had a working development environment and uploaded your first application to the ESP32. Now we will start exploring some of the more advanced functionality of the ESP8266, starting with the on board WiFi interface.

The ESP8266 has a built in WiFi interface that supports 802.11 b/g/n 2.4 GHz networking. 5GHz frequencies are only supported by the latest models (e.g. C5). The ESP32 can be setup to be an access point or to join an existing Wireless LAN. We are going to join a LAN in the laboratory.

### Step 1 - Load an Example Sketch

In the Arduino IDE, load the WiFiScan example sketch, using *File* -> *Examples* -> *WiFi* -> *WiFiScan* then upload the sketch to your ESP8266. This sketch will scan for local WiFi networks and display the results.

### Step 2 - Run the Sketch and Monitor Output

![Serial Monitor](../images/SerialMonitor.png)

To display the results, the sketch is using the Serial interface on the ESP32 to send output to. This output will be sent back into the USB port on your laptop/workstation. To see the output you need to open up the Serial Monitor, which is the magnifying glass icon at the top right of the IDE. You must ensure that the baud rate set in the serial monitor matches the speed of the `Serial.begin(115200);` statement in the setup function in the sketch.

Every 5 seconds you will see the board scan for available networks then output what it finds to the serial port.

### Step 3 - Extend your script

If you finish this assignment early modify the sketch to show the channel number each network is using and the encryption type in addition to the SSID and RSSI(signal strength).

### Step 4 - How to Connect to a WiFi Network

The example sketch **WiFiClient** shows how to connect to a WiFi network, providing a SSID and password, which we will use in part 2 of the workshop. Load the sketch (*File* -> *Examples* -> *WiFi* -> *WiFiClient*) and examine the code, take note of how the WiFi network credentials are entered to join a network.

You can use this step to connect to a hotspot you create with your phone. 
Registered devices will be able to connect to the HWResearchNetwork WiFi but for this you need to further modify your script or write a dedicated script to find the MAC address of your device. You can submit the MAC address on Canvas and we will request IT to register your device.

### Step 5 - Understanding the Pattern of Using the ESP8266 Library

Now you have seen 2 different example sketches using both Serial and WiFi connections. You may begin to see a pattern on how to use the resources:

- Optionally include the required header, such as `#include "WiFi.h"`
- In the **setup()** function initialise the library, usually with a begin() call and/or setting parameters
- In the **loop()** function access features of the library

If you finish early, jump back to Step 3 to add the additional functionality

---

[Click to return to the Part 1 homepage.](https://care-group.github.io/ESP866-IoT-Workshop/docs/part1/)
