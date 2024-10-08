# Your first ESP32 application

## Lab Objectives

This lab will show you how to use the Arduino IDE with the ESP8266 plugin to create a new application for the ESP8266 board. You will learn how:

- Ensure you have the correct settings in the IDE
- How to compile source code into a binary
- How to upload a binary onto the board
- You should also take time to understand the flow of an Arduino application, and the purpose of the **setup()** and **loop()** functions.

### Step 1 - Setting up the Arduino Environment for the ESP32

Make sure you setup your Arduino IDE to the correct port where you have connected your device and with the correct settings for the specific board you have been given. Using the *Tools* menu.

### Step 2 - Loading an Example Sketch

Then choose *File* -> *Examples* -> *ESP832* -> ... from the menu to open a new window with the sample sketches preloaded (you can close the previous window as it is not needed).

### Step 3 - Compiling the Sketch

![Verify command](../images/verify.png)

You can now compile the sketch you want to test for the ESP32 by selecting the **Verify** button on the command bar (tick icon) or using the *sketch* -> *Verify/Compile* menu option. You will see there are keyboard short cuts for various commands, shown next to the menu option which you may want to learn and use.

### Step 4 - Uploading the Sketch

![Upload command](../images/upload.png)

Once the compile has finished you can upload the new application to your ESP326 using the **upload** button on the command bar (arrow to right icon) or using the *Sketch* -> *Upload* menu option.

Yyou don't need to compile then upload. Just using upload will compile the application if necessary then upload it to the device.

If you try to save the sketch you will be prompted to enter a name/location for the sketch. This is because the example sketches are read-only, if you want to modify them and save the modification you need to save it to a new location.

This example sketch prints out information about the flash memory on board the ESP32. To see the output you need to open up the Serial Monitor.

![Serial Monitor](../images/SerialMonitor.png)

Ensure the baud rate at the bottom of the monitor window matches the baud rate in the setup function of the sketch `Serial.begin(115200);`.

### Step 5 - Understand the Example Sketch

The Arduino IDE and runtime take care of the work needed to setup the runtime for an application and provides 2 entry points. A **setup()** function, which is called at the start of the application, or when the device comes out of a deep sleep. The other entry point is the **loop()** function which is repeatedly called so long as the device is running.

There is no operating system running under the Arduino application, the code you enter in setup and loop is all that is running on the ESP8266 CPU.

This example sketch initialises the Serial connection in the **setup()** function then retrieves and prints information about the flash memory to the Serial console in the **loop()** function. At the end of the **loop()** function there is a delay for 5 seconds (5000 milliseconds). After the delay the **loop()** function ends, but is immediately called again.

---

[Click to return to the Part 1 homepage.](https://care-group.github.io/ESP32-IoT-Workshop/docs/part1/)
