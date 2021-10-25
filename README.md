# Welcome to the ESP8266 IoT Workshop

Welcome to the B31TF IoT Workshop based on ESP8266, a DHT11/22 and NeoPixel RGB LED.

This material is designed to guide you in your B31TF Sensors, Actuators &amp; IoT **individual assignment**.

The material in this workshop is based on the IBM workshop at this link (no longer being mantained): [https://binnes.github.io/esp8266Workshop/](https://binnes.github.io/esp8266Workshop/)

## Workplan 

The assignment is split into 5 parts.  A typical workplan is shown below:

| Week       |  Part                            |
|------------|----------------------------------|
| Week 7     |  [Part 1](docs/part1/README.md)       |            
| Week 8     |  [Part 1](docs/part1/README.md)       |
| Week 9     |  [Part 2](docs/part3/README.md)       |
| Week 10    |  [Part 2](docs/part2/README.md)       |
|            |  [Part 3](docs/part3/README.md)       |
| Week 11    |  [Part 4](docs/part4/README.md)       |
| Week 12    |                                  |
| Week 13    |  Final report due!               |

To start the workshop navigate to the [ Part 1 Introduction](docs/part1/README.md).

# Important Information

### Stream Environmental Conditions to The Cloud

You will learn how to connect an ESP8266 to a Cloud IoT platform over MQTT and stream environmental data from the sensors to a Jupyter notebook.

This document is designed to guide your learning esperience and help you to develop your system, gradually.
Make sure you understand each step and that each step works correctly, before moving to the next step.

The document provides information on what you should achieve at the end of each part, and lab sessions to let you familiarise with some of the hardware and software tools you will be using.
It also provides hints and links to useful resources and to some of the code you need to use, but does not give you a fully working solution. 
You will need time to research practical ways to achieve your objectives, and learn about ESP8266 programming, MQTT and IoT Platforms.

!!! note
  In addition to a MQTT broker, your application will use a number of services, such as Node-RED, to visualise sensor data and to control your device, and Jupyter, to carry out some simple data analytics.

  The original version of this workshop was based on IBM IoT and IBM Watson, but those platforms have been updated. In addition, while they provide a free-tier access, they require 
  credit card details for the registration.

  IMPORTANT: For this workshop you are only allowed to use free-to-use platforms that do not require credit card for registration.

  We have provided  [**here**](RESOURCES.md) some suggestions on free-to-use MQTT brokers and Cloud platforms you can use to connect and test the different parts of your applications.

  Alternatively, you may find other suitable MQTT brokers or IoT Cloud platforms providing their own MQTT broker as well as additional services, all-in-one, which may be convenient
  as you would not need to setup multiple accounts and use different dashboards for different services, e.g. see https://www.record-evolution.de/en/6-free-iot-platforms-to-start-your-iot-project-in-2021/
  and other links provided on Canvas. 

## Navigation

To move through the different step of the assignment you can use the side panels to select a specific section or use the navigation links at the bottom of each page to move to the next or previous section as required.

## Access to workshop material
Alternatively, you may find other suitable MQTT brokers, including MQTT brokers incorporated in IoT Cloud platforms providing different services all-in-one, which may be convenient
as you would not need to setup multiple accounts and use different dashboards for different services.
The source for this workshop is hosted on [GitHub](https://github.com/binnes/esp8266Workshop) and this site is automatically generated from the Markdown in the GitHub repository.

## Suggested work plan for the assignment

A suggest work plan can be seen [here](AGENDA.md).

## Course outline

### [Part 1](part1/README.md)

Make sure you have all the [prerequisite](part1/PREREQ.md) software installed, before you start.

Provides an overview to the assignment, introduces the hardware, the development tooling and then gets you programming the ESP8266 device to connect to the local WiFi network and be able to control the hardware.

### [Part 2](part2/README.md)

The second part of the assignment requires you to look at how you can connect a device to the Cloud using the MQTT protocol.  
Optional: You could investigate how to ensure a secure connection between the device and your cloud platform, using SSL/TLS security and certificates.

### [Part 3](part3/README.md)

In this section we look at using a low-code cloud-based development environment called Node-RED to implement the server side part of the IoT solution.  
You will need to create a dashboard to visualise the IoT data and also provide controls to configure the ESP8266 device.  
Your server side application will also control the LED attached to the ESP8266.

### [Part 4](part4/README.md)

In this section of the workshop looks at how useful information can be extracted from the IoT data using analytics.  

### [Part 5](part5/README.md)

The last part of the workshop asks you to examine and improve the energy efficiency of your system.

We've provided all the links used throughout the workshop as well as links to other resources [**here**](RESOURCES.md) to help you explore a little more about IoT.
