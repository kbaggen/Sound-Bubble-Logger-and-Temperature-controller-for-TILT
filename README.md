# Sound-Bubble-Logger-and-Temperature-controller-for-TILT


The Bubble-Logger is an Arduino device (ESP32) there monitor your fermentation by sound in regards of yeast activity though motioning CO2 blops pr. minute (BPM). Furthermore it repreats the TILT data of gravity and temperature though Bluethoth connection and hence display this information into Ubidots, Brewersfriend  or Brewfather. Last, and unike, it can also control a heating-argent and cooler based on the temeprature reading of the TILT every 2 min.

The software can be used though "Sum BLOPS(pt)/L" to give an indicative rG (reduction in gravity) estimate though polymnomial approach based on the use of same S-airlock, same calibrated sensor is used with a precise amount of water (4-4,5 ml)! In this way the Bubble-Logger emualted a PLAATO. For more detalis see www.bubble-logger.com

Hence, this project measure/do:

1. the activity of the yeast as CO2 escape the fermenter by a digital sound detector.
2. Repreats temperature by TILT
3. Rpreats gravity by TILT
4. Send data to the cloud in a easy way
5. Estimate of “reduction in gravity” (rG) can be calculated from the Sum BLOPS(pt)/L based on complex model taking pressure and temperature data into account. In this way the Bubble-Logger emualted a PLAATO, but need user interaction for creating polynomial to translate into a real SG estrimate. 
6. A 2-channel Relay to control a heat and cool source based on the temperature reading of the TILT every 2 min, hence, slow-working heating actor should be used.

It needs an ESP32 devkit, Sound Sensor Detecting Module LM393 and a 2-channel relay. Some 3 pins wires and some 2 pin wires is also needed.

Pin/pinout:
* heating: RELAY0_PIN 26  --> int1 on relay
* cooling: RELAY1_PIN 25  --> int2 på relay
* Soundsensor is connected on PIN 13
* 5v by ESP32 WIN
* Ground

Installing/Burn:
Please use Brewflasher! "Sound-Bubble-Logger-and-Temperature-controller-for-TILT" should be an option to burn though Breflasher.
https://github.com/thorrak/brewflasher/releases/tag/v1.0.1


If you wish to burn bin-files from scratch:
* 0xe000  boot_app0.bin 
* 0x1000  bootloader_qio_80m.bin 
* 0x10000 SBL4TILT_TempControl_v1.0.ino.bin 
* 0x8000  SBL4TILT_TempControl_v1.0.ino.partitions.bin 
USe "Flash Download Tools (ESP8266 & ESP32 & ESP32-S2)" https://www.espressif.com/en/support/download/other-tools
