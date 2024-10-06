# Workshop Summary

You have now completed the workshop.  I hope you enjoyed working through the various sections building up to the final solution.

Hopefully you now have a good understanding of some of the work needed to create an IoT solution.  In this workshop you:

- Constructed the hardware circuit using an ESP32 NodeMCU board with a DHT sensor and RGB LED
- Implemented the embedded C program for the ESP32 board:
  - Connected to local WiFi network
  - Handled communication with the connected DHT sensor and LED
  - Obtained the current time using NTP server
  - Connected securely to the Watson IoT platform
  - Sent and received JSON messages over MQTT
  - Implemented the Logistic Regression function with the parameters from the trained model
- Created SSL certificates to secure communication between the ESP32 board and a MQTT broker 
- Deployed an application and services on the Cloud, using a number of free-tier services (e.g. HiveMQ/Mosquitto public broker, Free-Node-RED, Jupyter on datacamp)
- Implemented Node-RED flows to work with IoT data, store data in a Cloudant NoSQL database and send commands to the ESP32 board
- Implemented a dashboard in Node-RED to visualise the IoT data and configure behaviour of the ESP32 board
- Worked in Jupyter to access database records, containing data from the ESP32 device
- Trained a classifier model inJupyter using data from the ESP32
- Validated the classifier model in Jupyter
- Extracted the model parameters from the Jupyter Notebook and implemented the model on the ESP32 to provide real-time classification of IoT data

This is just a taster of the many skills needed to implement an IoT solution.   Below you will find links to sources of continued learning, where you can explore in more depth some of the topics you touched in this workshop:

To go beyond the basics of Node-RED there is additional material available on the [Node-RED website](https://nodered.org) and there is a [workshop](https://github.com/binnes/moreNodeRedWorkshop) to learn more about Node-RED features
