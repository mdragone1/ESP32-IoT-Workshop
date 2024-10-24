# Controlling the RGB LED from the ESP32

## Lab Objectives
In this lab you will connect the NeoPixel LED and learn how to control it from the ESP32.  You will learn:

- The electrical connections of the LED and how to connect to the ESP32
- How to control the LED - by the end of the lab you should have written a program that can set the LED to specific colors.

NOTE: The LED we are using this year (2024/25) is not a NeoPixel but a RGB LED Common Anode from Switch Electronics
See link: https://www.switchelectronics.co.uk/products/rgb-5mm-led-common-anode?currency=GBP&variant=45356647678261&stkn=bbf1d20e1ed7&gad_source=1&gbraid=0AAAAAqEgT0D7UvQVtPwsikIBhB7H4bFe1&gclid=Cj0KCQjw4Oe4BhCcARIsADQ0cslP3T6Gr8zZ9_26iMx1aVwpPH4Cr72SDgCGR1ZSa1of0CRGnvDoInkaAjYbEALw_wcB


## Suggestions

During the last practical we used the ESPWiFi library that was installed as part of the plugin but now we want to control a RGB LED that we will connect to the ESP32. 

There is plenty of information online on how to use this type of RGB LEDs, for instance
https://www.hackster.io/techmirtz/using-common-cathode-and-common-anode-rgb-led-with-arduino-7f3aa9

If you have trouble connecting The TA will show you how to do that in the laboratory.
You could also try to find a suitable library that you could use to control the LED, so you won't need to implement the low-level control protocol for a device.

You need to be careful to ensure that any library you choose supports the hardware you are running on. Initially the Arduino platform was created around AVR microprocessors and many libraries were written to directly interact with AVR processors, so are incompatible when targeting a different processor. 

### Step 1 - Connecting the RGB LED to the ESP32 board

Now you need to connect the RGB LED to the ESP32. Before you start making any connections please disconnect the device from your laptop/workstation so there is no power getting to the device. You should never make any connection changes when the device is powered on.

Before making the connections we need to identify the 4 connecting pins coming out of the LED. If you examine the rim of the pixel cover you will see that one side is flattened (this should be the opposite side from the shortest pin) - this pin next to the flattened side is the **Data Out** pin. We will not be using this pin, as we only have a single pixel. You can chain pixels together connecting the **Data Out** pin to the **Data In** pin of the next pixel in the chain.

The shortest pin on the Pixel is the **Data In**
The longest pin on the Pixel is **Ground**
The remaining pin is **+'ve voltage**, which should be 5v, but it works with 3.3v that the ESP32 board provides.

So, with the shortest pin on the left and the flat side on the right the pinout is (left to right):

- Data In (shortest pin)
- +'ve Voltage
- Gnd (longest pin)
- Data Out (no connection)

You need to connect the Data In, +'ve voltage and ground to the ESP32 board.

IMPORTANT: You need to consult the pinout and schematic documentation for your bioard, to ensure that you are using the right connections.

CONNECTING THE WRONG PINS CAN DAMAGE THE ESP32 BOARD AND/OR THE LED !

### Step 2 - Write and Test your program,

If you have found a suitable library this may also offer an example sketch that you could use as a starting point for your
For example, in past years we were using a NeoPixel from AdaFruit, which provided an example sketch **strandtest** (*File* -> *Examples* -> *AdaFruit Neopixel* -> *strandtest*).

If you cannot find a suitable library, or prefer to implement the control of the LED yourself, you can follow an online tutorial or ask the TA for suggestions.

As a way of example, the following are the suggestions for how to modify the example program for the NeoPixel.

1. Change the LED_PIN number to match the GPIO you want to use in your device. Make sure you consult the schematic and the pinout documentation for your board.
2. Set the number of pixels to 1 in the LED_COUNT definition
3. In the loop function comment out the 4 lines starting with **theaterChase** as these cause rapid flashing when only a single LED is connected, which is not pleasant to look at

When you save the file you should be prompted to save it as a new file (you cannot override example files, so need to save them to another location to be able to modify them).

Compile and upload the sketch to see the LED change colours.

The top of your code should look like this:

```cpp
#include <Adafruit_NeoPixel.h>
#ifdef __AVR__
 #include <avr/power.h> // Required for 16 MHz Adafruit Trinket
#endif

// Which pin on the Arduino is connected to the NeoPixels?
// On a Trinket or Gemma we suggest changing this to 1
// Make sure to check and change the following to match a suitable GPIO for your device
#define LED_PIN    5

// How many NeoPixels are attached to the Arduino?
#define LED_COUNT 1

// Declare our NeoPixel strip object:
Adafruit_NeoPixel strip(LED_COUNT, LED_PIN, NEO_GRB + NEO_KHZ800);
// Argument 1 = Number of pixels in NeoPixel strip
// Argument 2 = Arduino pin number (most are valid)
// Argument 3 = Pixel type flags, add together as needed:
//   NEO_KHZ800  800 KHz bitstream (most NeoPixel products w/WS2812 LEDs)
//   NEO_KHZ400  400 KHz (classic 'v1' (not v2) FLORA pixels, WS2811 drivers)
//   NEO_GRB    Pixels are wired for GRB bitstream (most NeoPixel products)
//   NEO_RGB     Pixels are wired for RGB bitstream (v1 FLORA pixels, not v2)
//   NEO_RGBW    Pixels are wired for RGBW bitstream (NeoPixel RGBW products)


// setup() function -- runs once at startup --------------------------------

void setup() {
  // These lines are specifically to support the Adafruit Trinket 5V 16 MHz.
  // Any other board, you can remove this part (but no harm leaving it):
#if defined(__AVR_ATtiny85__) && (F_CPU == 16000000)
  clock_prescale_set(clock_div_1);
#endif
  // END of Trinket-specific code.

  strip.begin();           // INITIALIZE NeoPixel strip object (REQUIRED)
  strip.show();            // Turn OFF all pixels ASAP
  strip.setBrightness(50); // Set BRIGHTNESS to about 1/5 (max = 255)
}


// loop() function -- runs repeatedly as long as board is on ---------------

void loop() {
  // Fill along the length of the strip in various colors...
  colorWipe(strip.Color(255,   0,   0), 50); // Red
  colorWipe(strip.Color(  0, 255,   0), 50); // Green
  colorWipe(strip.Color(  0,   0, 255), 50); // Blue

  // Do a theater marquee effect in various colors...
  //theaterChase(strip.Color(127, 127, 127), 50); // White, half brightness
  //theaterChase(strip.Color(127,   0,   0), 50); // Red, half brightness
  //theaterChase(strip.Color(  0,   0, 127), 50); // Blue, half brightness

  rainbow(10);             // Flowing rainbow cycle along the whole strip
  //theaterChaseRainbow(50); // Rainbow-enhanced theaterChase variant
}
```


---

[Click to return to the Part 1 homepage.](https://care-group.github.io/ESP32-IoT-Workshop/docs/part1/)
