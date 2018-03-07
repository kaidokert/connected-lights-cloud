## Introduction
So, you have an objective to know “how to do IoT” and you need to know what's involved but in a way that can be easily adapted to any project.  Conveniently we have created a which will get you up and running by the end of the day.  For your convenience there is a free account tier of Mbed Cloud which is ideal for learning how it works before scaling out, in depth info about the Mbed Cloud product can be found [here](https://cloud.mbed.com/).

The chapters are organised in order of how an IoT gadget should be developed from choosing hardware to webapp design and then how to prepare the software for mass producing large volumes with minimal effort using our factory tools.
This tutorial is based on development board hardware, for anyone new to electronics its worth knowing your fancy smartphone in its shiny case was probably developed on ugly looking development boards about the size of your monitor before being refined to a hand held thing of beauty.  If something the size of your monitor can easily be shrunk down to a phone size object, imagine the tiny devices you will be able to create from the microprocessor platforms used in these tutorials; perhaps a new wearable, tiny environment sensor or ….  For seasoned product developers you will be able to quickly see how Mbed IoT solutions are structured from sensor to webapp and how it will accelerate your development cycles in upcoming IoT products.

If at any point you get stuck during the tutorials or need extra advice please contact us here iotsupport@arm.com or on the [forum](https://os.mbed.com/forum/) / [forum](https://forums.mbed.com/), we will use this feedback to enhance the tutorials with FAQ and tips which will help everyone in the Mbed community.

This tutorial is intended to give a quick introduction showing how to use the Mbed suite of products in a true end-to-end solution.  It isn’t intended to teach software engineering but will provide a few hints and tips along the way.

After learning how to build an embedded application and connecting it to the cloud we progress on to connecting a real webapp to Mbed Cloud for analytics and interaction.  As some of this is new to many of the audience we also provide help with necessary account creation as and when required.

Note, mbed supports many hardware platforms, some have integrated connectivity while others are standalone microprocessors. It goes without saying, for IoT applications you will need a development board that is capable of supporting both Mbed OS, connectivity and Mbed Cloud.  A few examples of appropriate hardware with combined processor and communication functionality are suggested for simplicity, advanced users may wish to mix and match combinations of microcontroller and radio but that’s beyond the scope of this particular tutorial.

We are always excited to hear about mass produced products using Mbed.  Please let us know when your device is approaching mass production stage so we can list it in our hall of fame and show it off at our extensive list of tradeshows around the world.  Contact Phill <__need a hall of fame email address__> to reserve your free Hall of Fame <__add link to HoF when its is live__> page.  Let’s get started….

## Tutorial: Building an internet connected lighting system
In this tutorial, you'll use the ARM Mbed IoT Device Platform to build a complete connected lighting system that you can deploy in your house or office. The system consists of one (or more) extra-bright RGB LEDs hooked up to an Mbed OS development board. A motion sensor can turn the lights on and off. The lights are also connected to the internet, so you can remotely change their color or state. The system uses Mbed Cloud to connect to the internet. It is secured end-to-end, and you can update it through over-the-air firmware updates.

You'll learn all the steps required to build the hardware, the cloud integration and the application. At the end of the tutorial, you'll have a firm understanding of building complete connected IoT solutions on the Mbed IoT Device Platform. You'll also have a cool light.

### Requirements
You will need hardware to follow along with the tutorial, here is a shopping list with  links to example distributor pages but the components are available in many other locations.  {Note the prices for 1-off hand assembled components is far higher than would be expected for your mass produciton design.}

 

* A [development board](https://developer.mbed.org/platforms/?software=16) capable of connecting to Mbed Cloud. Either:
    * [NXP FRDM-K64F](https://developer.mbed.org/platforms/FRDM-K64F/) using Ethernet. [Mouser](https://www.mouser.co.uk/ProductDetail/NXP-Freescale/FRDM-K64F?qs=sGAEpiMZZMtp5ziQ9mm%252bAhVeZ5z3voaQiWi991fCKtk=),  [Amazon](https://www.amazon.com/s/ref=nb_sb_noss_2?url=search-alias%3Daps&field-keywords=k64f)
    * [u-blox EVK-ODIN-W2](https://developer.mbed.org/platforms/ublox-EVK-ODIN-W2/) with Ethernet and Wi-Fi. [ublox](https://www.u-blox.com/en/product/evk-odin-w2)
    * [ST NUCLEO-F429ZI](https://developer.mbed.org/platforms/ST-NUCLEO-F429ZI) using Ethernet.[digikey](https://www.digikey.co.uk/product-detail/en/stmicroelectronics/NUCLEO-F429ZI/497-16280-ND/5806777), [Arrow](https://www.arrow.com/en/products/nucleo-f429zi/stmicroelectronics)
* A breadboard to hook up the components.[Sparkfun](https://www.sparkfun.com/products/12002), [Seeed}(https://www.seeedstudio.com/Bread-board-Clear-8.2-x-5.3cm-p-262.html)
* A micro-SD card - FAT formatted. [Amazon](https://www.amazon.co.uk/Kingston-Class10-microSDHCUHS-I-microSDHCto-Included/dp/B0162YQEIE/ref=sr_1_1?s=electronics-accessories&ie=UTF8&qid=1520423172&sr=1-1&keywords=micro+sd+card)
* A PIR sensor to detect motion.<__Jan to suggest a link for a known good component__>
* An RGB LED - preferably an extra-bright one.
    * For a better effect, you can also use a [Grove Chainable LED](http://wiki.seeed.cc/Grove-Chainable_RGB_LED/).
    * After building the original application, you can exchange the LED for something [fancier](https://www.adafruit.com/product/1138).
* Jumper wires, both male-male and male-female. [Seeed](https://www.seeedstudio.com/1-pin-dual-female-jumper-wire-100mm-50pcs-pack-p-260.html),[Seeed](https://www.seeedstudio.com/Breadboard-Jumper-Wire-Pack%28241mm-200mm-160mm-117mm%29-p-234.html)
* Resistors: 1x 100 Ohm, 2x 220 Ohm. [Amazon](https://www.amazon.co.uk/Carbon-Resistor-0-25w-100-100R/dp/B004S12JLK/ref=sr_1_3?ie=UTF8&qid=1520430420&sr=8-3&keywords=100+ohm&dpID=31Dmlw9MiuL&preST=_SY300_QL70_&dpSrc=srch), [Amazon](https://www.amazon.co.uk/Sonline-Through-Hole-Carbon-Resistors/dp/B00X9HRKKY/ref=sr_1_3?s=electronics&ie=UTF8&qid=1520430454&sr=1-3&keywords=220+ohm)

__Change photo to an Odin__

<span class="images">![Components needed](https://s3-us-west-2.amazonaws.com/cloud-docs-images/lights2.png)<span>Components required to build our lighting system. Top row: RGB LED, PIR sensor, Grove Chainable LED. Bottom row: breadboard, NXP FRDM-K64F, jumper wires.</span></span>

You also need:

* [An account](https://portal.us-east-1.mbedcloud.com) to access to the Mbed Cloud Portal.
