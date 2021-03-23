# "SBL4TILT" - Sound-Bubble-Logger-and-Temperature-controller-for-TILT
(DISCLAIMER: this software are still in beta and hence treat it carefully).

The Bubble-Logger is an Arduino device (ESP32) there monitor your fermentation by sound in regards of yeast activity though motioning CO2 blops pr. minute (BPM). Furthermore it repeats the TILT data of gravity and temperature though Bluetooth connection and hence display this information into Ubidots, Brewersfriend  or Brewfather. Last, and unique, it can also control a heating-argent and cooler based on the temeprature reading of the TILT every 2 min.



Hence, this project measure/do:

1. Measure the activity of the yeast as CO2 escape the fermenter by a digital sound detector giving blops pr. min (BPM) and Sum BLOP(pt)/L). .
2. Repeats temperature and Gravity from TILT into cloud.
3. Hence, Send all data to the cloud in a easy way (BPM, Sum BLOP(pt)/L, Temperature, Gravity and color of TILT in use). The software sends to Ubidtos, Brewfather and Brewersfreind if you enter the url or Token in captive portal mode.
4. It can control a 2-channel Relay to control a heat and cool source based on the temperature reading of the TILT every 2 min, hence, slow-working heating actor should be used (I used a 30W reptile heating mat).


![alt text](https://github.com/kbaggen/Sound-Bubble-Logger-and-Temperature-controller-for-TILT/blob/master/pic/SBL4TILT_outcome.png)
https://web.brewfather.app/share/vcbmFVVWhc2ZLq

### Questions/contact?

Facebook group: https://www.facebook.com/groups/2176394599141882

## Buidling a "SBL4TILT"
This build is rather eaasy and do not need any soldering as such. It do need good knowledge for mains and hence, all high voltage work should be accomblished by a skilled worker in this area.

### Needed parts and Pin/pinout
**It needs an ESP32 devkit, Sound Sensor Detecting Module LM393 and a 2-channel relay. Some 3 pins wires and some 2 pin wires is also needed.**
* heating: RELAY0_PIN 26  --> int1 on relay
* cooling: RELAY1_PIN 25  --> int2 på relay
* Sound sensor is connected on PIN 13
* 5v by ESP32 WIN
* Ground

A "light" version can be made by not adding the relay part to the build. In this state you only need provide power by WIN, ground and GPIO 13 to be attached till the sound sensor. If you new to Arduino this might be the place to start and hence later take a look at include the relay :-)

![alt text](https://github.com/kbaggen/Sound-Bubble-Logger-and-Temperature-controller-for-TILT/blob/master/pic/esp32_SBL4T_TempControl2.png)

### Relay and warning = beta software
This software is in beta and even I have included all the safety I can think off please treat it carefully. The Relay do turn off if no TILT is found, and the heating part of the relay do turn off before each data treatment to secure the heating is off if it goes down. The Bubble Logger do not support temperatures below 2´C or over 60´C and will turn relay of if set outside this range. If the wifi is unstable, and hence if the logger lose wifi it will restart to secure a new connection, hence, there is build in a behavior of restarting. If you experience any melt-down where the logger get stuck in either heating or cooling mode, please, let me know!

### Building Sound sensor with “condom” and placement in airlock.
(Fitting the Condom – Water Balloon on the LM393 – fitting in Airlock – Alignment)
The LM393 need a moisture protection, and this is done by a small water balloon, and it should be rather tight around the noose, but still loose as below pictures shows. It needs to sit tight in the airlock making an seal to restrict any water from vaporization. To allow the pressure to equalize a small hole needs to be drilled. Align it so the micorphone is place over the direct hole in the airlock, so the sensor get the direct sound “blop”.
As the sensor got some shapes edges there will flence the balloon and secondly as the microphone rather easily can break off, try steady the sensor by some tape as first picture shows!
![alt text](https://github.com/kbaggen/Sound-Bubble-Logger-and-Temperature-controller-for-TILT/blob/master/pic/buidling%20sensor.png)

Notice the small hole in airlock. This helps filling and clean the airlock if you choose not to ever remove the s-airlock (is the case of a blow-out system).

Besides the water amount of 4-4,5ml and the use of a calibrated censor the foremost important factor is the alignment of the probe, and it need to be pressed all the way down in the s-airlock and aligned directly over the tube-hole and hence get the direct release of C02 sound/pressure-burst. If not fitted precisely you loose BPM and hence the rG estimate goes wrong if you choose to make use of the polynomial approach as second opinion of SG estimation.

### Calibration.
Make a brew and Put on the “condom” (small water balloon) on the LM393, se below picture! When the BPM is around 15-30 (in avenge over 20-30min) by hear and see count adjust the potentiometer of the sensor till it reflect this by the logger (simply turn the potentiometer down until it stops lighten green and then fine adjust until you get the same BPM as hear and see count) . This is best done at a pressure around 1010-1018 hPa as the bubble rate is easy to detect by hear and see count. This give you a calibrated LM393 ready for your next brew (if you do the calibration very early in the fermentation and a large brew it should also be useful during the brew calibration is done upon).


During the next 1-2 brews you can fine tune the potentiometer of the sound sensor so it reflects the BPM by hear and see. In all, during 3 brews you should hold a calibrated sensor there can give a precision of -/+ 3 gravity units if you make use of polynomial approach.

This give a high resolution sensor there miss a few and also post some double bubbles, but this is fine as long the avenge BPM do reflect you hear and see count. calibration should give you between 30 and up till 100-150 SBM at high krauzen depending on temperature/yeast/brew size (I brew in 14-25L amounts), etc! This setting is prone to high sounds, but light talking, music, drier and washing machine is ok to have nearby!

To be able to compare from brew to brew of BPM and hence make use of polynomial you should try to hold as many variable the same, e.g. same sensor from brew to brew and foremost have same amount of water in airlock (+ same kind of S-airlock). I use 4-4,5 ml. Secondly, ensure the alignment of the probe is the same from brew to brew. Consider to have a blow-out setup where sensor never leaves the S-airlock.

## Installing/Burn
Please use Brewflasher! "Sound-Bubble-Logger-and-Temperature-controller-for-TILT" e.g. "SBL4TILT" or "SBL4TILT-light" should be an option to burn though Brewflasher.
https://github.com/thorrak/brewflasher/releases/


## Operate/setup
The Bubble Logger got a captive portal mode and hence you log on just as it was a Wifi access point. If the login page do not automatically come up, go to: 192.168.4.1

It will for 60 seconds go into this “Captive Portal” at power on (or after any failures as lost connection or lost power) where all setting can be done (E.g hence, set Brew Name, SSID+Password, Brew Size, start Temperature, (License, currently not needed), TILT offset temperature/gravity and setting URL of either Brewfather URL/Ubidots Token/Brewersfriend). It light blue when in “Captive Portal” mode (and also blink blue when sending/treating data including detection of a bubble). 

Hence,
* Connect till the “Bubble Logger CONNECT” SoftAP though either you mobile phone or laptop by Wifi.  After connect till access point the Bubble-logger url for the configuration page is: 192.168.4.1 
* Set the various parameters accordingly (Currently, you need to set SSID+Password every time you enter “Captive Portal” mode, so you need to set SSID+password even you just changed one parameter. A bug we are working on).
* If you later on wish to change for instance temperature, pull power for 2 sec, and go into portal mode agian and change the temperature (remember to set SSID+password again too).

![alt text](https://github.com/kbaggen/Sound-Bubble-Logger-and-Temperature-controller-for-TILT/blob/master/pic/SBL4TILT_gui.png)

Secondly, the Bubble-Logger also got a webserver you can follow all data on during brewing, to access this on you need to find the IP either in you routing table or some Network sniffer program. The Webserver is on: 192.168.1.xxx:8080 (hence please notice the port is 8080).

You will ofcouse need a Ubidots STEM account and hence the TOKEN (see under API credentials), and for Brewfather you need to enable "Custom Stream" and inset this associated URL into Bubble-logger. I am not good a Brewersfriend, but this is similarly done as Brewfather.

### What data is send?
The following data is send till Brewfather and Brewersfriend (please notice I am not good at Brwersfriend and hence might be issues/error):
* "Blop pr. min" is send as: BPM
* "Sum BLOPS(pt)/L" is send as: Pressure (PSI)
* "Gravity" is send as: Gravity (G)
* "Temperature" is send as: Temperature (C)
* "TILT color" is send as a comment and can be seen under devices.

(Please notice I do not and do not intend to support other than metric numbers. 3rd worlds countries has to add up and follow the scientific rules.)

For Ubidots the above is also send, but also the power-state of relay is send, where the following coding is used.
* 0 = Relay awaiting.
* 1 = Relay cooling.
* 2 = Relay heating.
* 3 = Relay turned of as something wrong ~ No TILT?
* 4 = Relay turned of as SetTemp is outside supported range of 2-60´C.
