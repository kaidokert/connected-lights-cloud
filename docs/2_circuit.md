### Building the circuit



#### SD card

<span class="notes">**Note:** Before you start, put the SD card in the SD card reader on your development board.</span>

Mbed Cloud requires an external storage to store the identity of the device, and to store firmware updates. This external storage does not necessarily have to be an SD card (which is too expensive to put in a production device), you can also use internal flash or external SPI flash. To modify the storage layer, you'll need to modify the [bootloader](see 'Section 8 - Firmware updates') and the application (see `mbed_app.json` in the `connected-lights-cloud` application later in this tutorial); which is not covered in this tutorial. More information is in the [Mbed Cloud docs](https://cloud.mbed.com/docs/).  Our internal development boards mostly use a SPI [flash component](https://www.mouser.co.uk/ProductDetail/?qs=sz9oxq9AnY8q5F9zmlFAYA%3D%3D).

#### Finding suitable pins

For the circuit, you need four digital pins. Three of these need to support pulse width modulation (PWM). Using PWM, you can control the amount of power flowing through a circuit, and you can use this to dim the colors of the LED on one of the three RGB channels.

To find pins that you can use, look at the [platform page](https://developer.mbed.org/platforms/) for your board, and find the pinout. The pinout defines the characteristics of the pins. For example <__lets change this to Odin as it is actually mass producible__>, here is the pinout for the ~~FRDM-K64F~~Odin. In this example, you can use ~~D5~~, ~~D6~~ and ~~D7~~ as PWM pins:

<span class="images">![FRDM-K64F pinout showing PWM pins](https://s3-us-west-2.amazonaws.com/cloud-docs-images/lights3.png)<span>The pinout for the ~~NXP FRDM-K64F~~Odin, showing that you can use pins D5, D6 and D7 for PWM.</span></span>

You also need a pin for the PIR sensor. This can be any digital pin, as long as it's not marked as UART (D0 and D1 on the pinout above). In this example, you can use pin D2.

<span class="notes">**Note:** In general, it's a good idea not to use any of the I2C and SPI pins for LEDs and basic sensors because connectivity shields (such as Wi-Fi) might need them.</span>

#### Connecting the peripherals on a breadboard

Here is a diagram of hooking up the PIR sensor and the RGB LED to your board. Replace the pins D2, D5, D6 and D7 with the pins you found for your board. If you have a four-pin RGB LED, you must position the LED so that the longest pin is the second from the left. (Hold it [like this](http://howtomechatronics.com/wp-content/uploads/2015/09/RGB-LED.png?28ea0f).)

<span class="images">![PIR sensor and RGB LED Fritzing diagram](https://s3-us-west-2.amazonaws.com/cloud-docs-images/lights4.png)</span>

<span class="notes">**Note - common anode or common cathode LED:** Four pin RGB LEDs come in two different types: *common anode* and *common cathode*. If you have a common cathode LED, connect the second pin to `GND` instead of `3.3V`. If you are unsure, try both circuits and see which one works.</span>

<span class="notes">**Note:** If you're unsure of the pins on the PIR sensor and you have a sensor with a 'dome' on it, remove the dome. The PCB describes the pins.</span>

When you have connected everything, the circuit looks something like this:

<span class="images">![PIR sensor and RGB LED connected to FRDM-K64F](https://s3-us-west-2.amazonaws.com/cloud-docs-images/lights5.png)</span>


Note, the original draft of this page was written for the K64F but our marketing team thought a WiFi version would be more interesting at the risk of the authors having to rewrite the entire page.  As we already said, its very easy to jump between different platforms and the only changes were a new photo and different pin numbers as its physiclly a diffeent baord.  Oh and obviosuly no need for WiFi SSID and password when we get to that stage.  If you are interested in seeing the original K64F page see it <__here__>
