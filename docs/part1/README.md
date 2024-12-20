# Part 1

Before you can start working on this part you need to have some prerequisite software installed.

Details of how to get setup can be found on the [prerequisites page](PREREQ.md).

## Setup for Workshop

This section will take you through installing the prerequisite software on your machine.

- Estimated duration: 15 min
- Practical: [**Installing Prerequisites and Setup**](PREREQ.md)

## Introduction to the ESP32

This lab exercise will show you how to use the Arduino IDE with the ESP32 plugin to create a new application for the ESP32 board.

- Estimated duration: 10 min
- Practical: [**First App on ESP32**](FIRSTAPP.md)

## ESP32 WiFi Connectivity

This lab exercise will show you how to connection your ESP32 to a local WiFi network. This Lab will also introduce the Serial Monitor, which allows you to see output from a running application.

IMPORTANT NOTE: Eduroam and other WIFi networks managed by HWU (such as HWUResearch) will block some ports, crucially 1883 and the other ports needed by MQTT.
To solve the issue, you can create a network using your phone as hot spot. 
The IoT application you are going to build will use very little data but if you want a free network you can connect
your phone to the _The_Cloud, after creating an account on that service.

- Estimated duration: 15 min
- Practical: [**WiFi Scanning and Connectivity**](WIFI.md)

## LED Light - Sample App

In this lab exercise you will connect the NeoPixel LED and learn how to control it from the ESP32

- Estimated duration: 15 minutes
- Practical: [**RGB LED**](LED.md)

## DHT Temp - Sample App

In this lab exercise you will learn how to connect the DHT temperature and humidity sensor to the ESP32 board and how to access data from the sensor.

- Estimated duration: 15 minutes
- Practical: [**DHT Sensor**](DHT.md)
