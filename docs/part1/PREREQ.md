# Installing Prerequisites and Setup

## Lab Objectives

This lab will ensure you have all the resources and software needed to complete the lab installed. You should follow the instructions for your OS and complete all sections of the setup before moving forward with the Lab.

## ESP32 Development

To be able to complete the workshop you need install the required software to your laptop or workstation. 

### WiFi Connectivity

The ESP32 can connect to a 2.4GHz network supporting 802.11 b/g/n. Only the most recent models (e.g. C5) can also work with 5GHz frequencies (802.11 ac).

As there is no ability to launch a browser on the ESP32, so you cannot work with WiFi networks needing a browser to be able to enter credentials, which is a mechanism often used in public spaces, such as hotels.

The workshop does not support advanced authentication, such as using LDAP or certificates to authenticate to the network. You should have a network that uses an access token/password, such as WPA/WPA2 - this is what most home WiFi access points provide.

Many corporate networks are difficult to connect IoT devices to, as they can be tightly secured, often requiring certificates to be installed.

If a suitable network is not available then smart Phone hotspots can be used to provide connectivity. The workshop does not require large amounts of data for the ESP32, so is suitable for using a phone hotspot.

There are no incoming ports needed for the workshop, but the ESP32 needs to be able to connect via MQTT protocol over TCP to ports 1883 and 8883. The workshop also need web access over TCP ports 80 and 443. The final port that is needed is for Network Time Protocol (NTP), which uses an outbound UDP connection on port 123.

### Purchasing the required Hardware

You need the following hardware to work through the workshop:

- ESP32
- NeoPixel RGB LED (or any other chainable RGB/RGBW LED based on ws2812b or sk6812 chips ), such as [this from Adafruit](https://www.adafruit.com/product/1734) (Search for **Neopixel 8mm or 5mm** - often sold in packs of 5)
- DHT11 Temperature / Humidity Sensor (search for **DHT11 or DHT22**)
- 6 x Female to Female jumper wires (search for **dupont cable f2f or f-f** - usually sold in packs of 40 cables)
- MicroUSB cable (Please ensure it is a data cable, not just a power cable)

The workshop instructions uses the DHT11 temperature and humidity sensor. This can be replaced with the DHT22 sensor, which has the same pinout, but offers a more accurate sensor. DHT11 is accurate within 2C, whilst the DHT22 is accurate to within 0.5C.

## Installing the Required Software

The following instructions have been tested against Linux (Ubuntu 18.04LTS and Fedora 27), MacOS (High Sierra) and Windows 10. If you are using a different OS then you may need to adapt the instructions to match your installed OS.

You may need admin access to your workstation to be able to install the software and drivers.

### Step 1 - Install Drivers

If you are using the kit given to you in the B31OT laboratory, then the boards you will be using will need either the CP210x or the CH340 USB to serial chip. 

You should check the datasheet of your board to make sure you identify the right USB to UART communication chip that’s being used in your board.

You may need a driver for your OS to be able to communicate with the USB to serial CH340G chip used in the ESP32 modules. Do not plugin the device until you have installed the driver on Windows and Mac.

**macOS**

See also : https://randomnerdtutorials.com/install-esp32-esp8266-usb-drivers-cp210x-mac-os/


**Windows / Linux**

Link: [https://github.com/nodemcu/nodemcu-devkit/tree/master/Drivers](https://github.com/nodemcu/nodemcu-devkit/tree/master/Drivers)

Select the appropriate one for your OS, download it, unzip it and install it.

---
**IMPORTANT**

Linux should not need a driver installing, as it should already be installed.


**All**

When the driver is installed and the NodeMCU module is connected you can test if the driver is working:

- Linux : You will see a device appear from the command `ls /dev/ttyUSB*`
- MacOS : You will see an additional device appear from output of command `ls /dev/tty.*`
- Windows : You will see an additional COM port in the Ports section of the Device Manager.

### Step 2 - Install the Arduino IDE

The workshop will use the Arduino IDE to create applications for the ESP32 module. You need to have an up to date version of the Arduino IDE, available from [**here**](https://www.arduino.cc/en/Main/Software). Select the version for your OS then download and install it.

**Linux**

Your linux distro may have Arduino available in the software package manager catalog, if not you can manually install it:
- Unarchive it, move it to /opt or /usr/local (`sudo mv arduino-1.8.7 /opt`) then run `/opt/arduino-1.8.7/install.sh`;
- Some Linux distros you may need to add your user to the TTY and dialout groups to be able to use the connection to the device. You can do this using command `sudo usermod -a -G tty,dialout $USER` you will have to log out and log in again to get the added permissions.

---
**IMPORTANT**

You will need to change the version number in the command above if you downloaded a version newer than 1.8.7.

---

**macOS**

Simply drag Arduino app into Applications folder after unzipping.

**Windows**

Simply run the downloaded installer application.

### Step 3 - Install the ESP32 Plugin for Arduino IDE

Out of the box the Arduino IDE does not support ESP32 development. You need to add a plugin to add support. Launch the Arduino IDE then open up the preferences panel for the Arduino IDE:

- Linux : *File* -> *Preferences*
- MacOS : *Arduino* -> *Preferences*
- Windows : *File* -> *Preferences*

Paste in the URL for the ESP plugin to the *Additional Board Managers URLs* field: `https://espressif.github.io/arduino-esp32/package_esp32_index.json`

Select 'OK' to close the preferences dialog.

Open Boards Manager from Tools > Board menu and install esp32 platform (and do not forget to select your ESP32 board from Tools > Board menu after installation).

Once installed close the Board Manager. You may need to restart Arduino IDE before continuing.

### Step 4 - Install the Filesystem Upload Tool for ESP32 
IMPORTANT: You can skip this step as you do not need to implement a secure MQTT connection.
This step will be shown as a tutorial in class.
The instructions below do not work with new (>=2.0) Arduino IDE

The ESP32 has flash memory that can hold a filesystem. There is a plugin for Arduino that allows you to generate a populated filesystem and upload it to the ESP32 board. See here https://randomnerdtutorials.com/install-esp32-filesystem-uploader-arduino-ide/#installing where you can download and install the plugin. You need to create a tools directory within the sketch directory then extract the content there.


**IMPORTANT**

You can find the sketch directory location from the preferences panel of the Arduino IDE.

---

The default location of the sketch directory is:

- Linux - **/home/< user name >/Arduino/tools/ESP32LittleFS**
- MacOS - **/Users/< user name >/Documents/Arduino/tools/ESP32LittleFS**
- Windows - **C:\Users\< user name >\Documents\Arduino\tools\ESP32LittleFS**

#### Step 5 - SSL utility to work with certificates
IMPORTANT: You can skip this step as you do not need to implement a secure MQTT connection.
This step will be shown as a tutorial in class.
The instructions below do not work with new (>=2.0) Arduino IDE

During the workshop you will be generating your own self-signed certificates, so need the OpenSSL tooling installed. Follow the instructions for your OS below:

**Linux**

OpenSSL is installed as part of the OS for most distros, so should have nothing to do here. If it is not installed then most distros have an OpenSSL package which can be installed using the distro package installer tool.

**macOS**

OpenSSL is installed as part of the OS, so nothing to do here.

**Windows**

There are 2 options for installing OpenSSL on Windows. You can install a binary distribution to run on Windows or you can enable the Windows Subsystem for Linux, which provides a Linux environment within Windows:
- **Windows Binary**: the OpenSSL official website only provides source. You can choose to build the binaries from source, but there are links to sites hosting prebuilt binaries, such as [this site](https://slproweb.com/products/Win32OpenSSL.html) for 32 and 64 bit Windows. You want to select one of the 1.1.x versions. You only need light version for this workshop, but you can choose the full version if you want the additional developer resources. When installing, the default install options are OK. The standard install does **NOT** add the OpenSSL executable to the system PATH, so you will need to specify the full path of the binary when entering commands, unless you add it to the PATH, e.g. `c:\OpenSSL-Win64\bin\OpenSSL.exe`. 
- **Windows Subsystem for Linux**: this option installs a Linux distribution within Windows, so you get access to all the Linux utilities and can install additional packages, such as OpenSSL. To enable Linux Services for windows follow the instructions [here](https://docs.microsoft.com/en-us/windows/wsl/install-win10). Select Debian as the Linux distribution, then when it is installed launch Debian then run the following commands at the Linux command prompt:
  ```
  bash
  sudo apt-get update ; sudo apt-get upgrade
  sudo apt-get install openssl
  ```

---
**IMPORTANT**

The first method will not provide the xxd binary, but you don't need it for this workshop. If you get an error saying **MSVCR120.dll** is missing, then you can download the Visual Studio 2013 redistibutable package [here](https://support.microsoft.com/en-us/help/3179560).

---

[Click to return to the Part 1 homepage.](https://care-group.github.io/ESP32-IoT-Workshop/docs/part1/)
