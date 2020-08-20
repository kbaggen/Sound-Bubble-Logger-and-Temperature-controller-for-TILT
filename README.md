# Sound-Bubble-Logger-and-Temperature-controller-for-TILT


The Bubble-Logger is an Arduino device (ESP32) there monitor your fermentation by sound in regards of yeast activity though motioning CO2 blops pr. minute (BPM). Furthermore it repreats the TILT data of gravity and temperature though Bluethoth connection and hence display this information into Ubidots, Brewersfriend  or Brewfather. Last, and unike, it can also control a heating-argent and cooler based on the temeprature reading of the TILT every 2 min.

The software can be used though "Sum BLOPS(pt)/L" to give an indicative rG (reduction in gravity) estimate though polymnomial approach based on the use of same S-airlock, same calibrated sensor is used with a precise amount of water (4-4,5 ml)! In this way the Bubble-Logger emualted a PLAATO. For more detalis see www.bubble-logger.com

Hence, this project measure/do:

1. the activity of the yeast as CO2 escape the fermenter by a digital sound detector.
2. Repreats temperature from TILT into cloud
3. Rereats gravity from TILT into cloud
4. Hence, Send all data to the cloud in a easy way
5. Estimate of “reduction in gravity” (rG) can be calculated from the Sum BLOPS(pt)/L based on complex model taking pressure and temperature data into account. In this way the Bubble-Logger emualted a PLAATO, but need user interaction for creating polynomial to translate into a real SG estrimate. 
6. A 2-channel Relay to control a heat and cool source based on the temperature reading of the TILT every 2 min, hence, slow-working heating actor should be used.

**It needs an ESP32 devkit, Sound Sensor Detecting Module LM393 and a 2-channel relay. Some 3 pins wires and some 2 pin wires is also needed.**

### Pin/pinout:
* heating: RELAY0_PIN 26  --> int1 on relay
* cooling: RELAY1_PIN 25  --> int2 på relay
* Soundsensor is connected on PIN 13
* 5v by ESP32 WIN
* Ground

![alt text](https://github.com/kbaggen/Sound-Bubble-Logger-and-Temperature-controller-for-TILT/blob/master/esp32_SBL4T_TempControl.png)

### Installing/Burn:
Please use Brewflasher! "Sound-Bubble-Logger-and-Temperature-controller-for-TILT" should be an option to burn though Breflasher.
https://github.com/thorrak/brewflasher/releases/tag/v1.0.1

If you wish to burn bin-files from scratch:
* 0xe000  boot_app0.bin 
* 0x1000  bootloader_qio_80m.bin 
* 0x10000 SBL4TILT_TempControl_v1.0.ino.bin 
* 0x8000  SBL4TILT_TempControl_v1.0.ino.partitions.bin 

USe "Flash Download Tools (ESP8266 & ESP32 & ESP32-S2)" https://www.espressif.com/en/support/download/other-tools

## Buliding sensor and calibration
The digital Sound Sensor Detecting Module LM393 needs to be calibrated to a degree where it is responsive, but where we also can “work” besides make some noise.


### Building Sound sensor with “condom” and placement in airlock.
(Fitting the Condom – Water Balloon on the LM393 – fitting in Airlock – Alginment)
The LM393 need a moisture protection, and this is done by a small water balloon, and it should be rather tight around the noose, but still loose as above pictures shows. It needs to sit tight in the airlock making an seal to restrict any water from vaporization. To allow the pressure to equalize a small hole needs to be drilled. Align it so the mirc is place over the direct hole in the airlock, so the sesor get the direct sound “blup”.
As the sensor got some shapes edges there will flence the ballon and secondly as the mirc rather easily can break off, try steady the sensor by some tape as first picture shows!

Notice the small hole in airlock. This helps fillling and clean the airlock if you choose not to ever remove the s-airlock (is the case of a blow-out system).

Besides the water amount of 4-4,5ml and the use of a calibrated censor the foremost important factor is the alignment of the probe, and it need to be pressed all the way down in the s-airlock and aligned directly over the tube-hole and hence get the direct release of C02 sound/pressure-burst. If not fitted precisely you loose BPM and hence the rG estimate goes wrong if you choose to make use of the polynomial approach as second opion of SG estrimation.

### Calibration.
Make a brew and Put on the “condom” (small water balloon) on the LM393, se below picture! When the BPM is around 15-30 (in avenge over 20-30min) by hear and see count adjust the potentiometer of the sensor till it reflect this by the logger (simply turn the potentiometer down until it stops lighten green and then fine adjust until you get the same BPM as hear and see count) . This is best done at a pressure around 1010-1018 hPa as the bubble rate is easy to detect by hear and see count. This give you a calibrated LM393 ready for your next brew (if you do the calibration very early in the fermentation and a large brew it should also be useful during the brew calibration is done upon).


During the next 1-2 brews you can fine tune the potentiometer of the sound sensor so it reflects the BPM by hear and see. In all, during 3 brews you should hold a calibrated sensor there can give a precision of -/+ 3 gravity units if you make use of polynomail approach.

This give a high resolution sensor there miss a few and also post some double bubbles, but this is fine as long the avenge BPM do reflect you hear and see count. calibration should give you between 50 and up till 100-150 SBM at high krauzen depending on temperature/yeast/brew size (I brew in 14-25L amounts), etc! This setting is prone to high sounds, but light talking, music, drier and washing machine is ok to have nearby!

To be able to compare from brew to brew of BPM and hence make use of polynomial you should try to hold as many variable the same, e.g. same sensor from brew to brew and foremost have same amount of water in airlock (+ same kind of S-airlock). I use 4-4,5 ml. Secondly, ensure the alignment of the probe is the same from brew to brew. Consider to have a blow-out setup where sensor never leaves the S-airlock.
 

### One sensor = One Airlock
I propose you stick to use one airlock with you calibrated sensor especially if you have made you own polynomial and is using this setup for estimating rG/SG. My testing do propose there is not big different between airlocks, see this post, but still “one sensor = one airlock” is current best approach.
