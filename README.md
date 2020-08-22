# "SBL4TILT" - Sound-Bubble-Logger-and-Temperature-controller-for-TILT
(DISCLAIMER: this software are still in beta and hence treat it carefully).

The Bubble-Logger is an Arduino device (ESP32) there monitor your fermentation by sound in regards of yeast activity though motioning CO2 blops pr. minute (BPM). Furthermore it repreats the TILT data of gravity and temperature though Bluethoth connection and hence display this information into Ubidots, Brewersfriend  or Brewfather. Last, and unike, it can also control a heating-argent and cooler based on the temeprature reading of the TILT every 2 min.

The software can though the "Sum BLOPS(pt)/L" be used to give an indicative rG (reduction in gravity) estimate by polymnomial approach based on the use of same S-airlock, same calibrated sensor, known amount of wort,  and 4-4.5ml water in airlock! In this way the Bubble-Logger emualted a PLAATO. For more detalis see below!

Hence, this project measure/do:

1. Measure the activity of the yeast as CO2 escape the fermenter by a digital sound detector giving blops pr. min (BPM) and Sum BLOP(pt)/L). The Sum BLOP(pt)/L is the sum of Blops corrected for prressure and wort temerature impact on the bubbling rate and as it is corrected for brew size too, it is hence a comparable number from brew to brew (based on same calibrated sensor, aligment, water amount, airtight fermenter, excact known brewsize, etc.).
2. Repreats temperature from TILT into cloud
3. Rereats gravity from TILT into cloud
4. Hence, Send all data to the cloud in a easy way (BPM, Sum BLOP(pt)/L, Temperature, Gravity and color of TILT in use). The software sends to Ubidtos, Brewfather and Brewersfreind if you enter the url or Token in captive portal mode.
5. Estimate of “reduction in gravity” (rG) can be calculated from the Sum BLOPS(pt)/L based on complex model taking pressure and temperature data into account. In this way the Bubble-Logger emualted a PLAATO, but need user interaction for creating polynomial to translate into a real SG estrimate. 
6. A 2-channel Relay to control a heat and cool source based on the temperature reading of the TILT every 2 min, hence, slow-working heating actor should be used (I used a 30W reptile heating mat).

![alt text](https://github.com/kbaggen/Sound-Bubble-Logger-and-Temperature-controller-for-TILT/blob/master/pic/SBL4TILT_outcome.png)
https://web.brewfather.app/share/vcbmFVVWhc2ZLq


### Needed parts and Pin/pinout
**It needs an ESP32 devkit, Sound Sensor Detecting Module LM393 and a 2-channel relay. Some 3 pins wires and some 2 pin wires is also needed.**
* heating: RELAY0_PIN 26  --> int1 on relay
* cooling: RELAY1_PIN 25  --> int2 på relay
* Soundsensor is connected on PIN 13
* 5v by ESP32 WIN
* Ground

A "light" version is also offered and in this version all related to "relay" and temperature control is turned off. This versipon only needs VCC on WIN, ground and GPIO 13 to be attached till the sound sensor. If you new to Arduino this might be the place to start :-)

![alt text](https://github.com/kbaggen/Sound-Bubble-Logger-and-Temperature-controller-for-TILT/blob/master/pic/esp32_SBL4T_TempControl2.png)

### Installing/Burn
Please use Brewflasher! "Sound-Bubble-Logger-and-Temperature-controller-for-TILT" should be an option to burn though Breflasher.
https://github.com/thorrak/brewflasher/releases/tag/v1.0.1

If you wish to burn bin-files from scratch (see releases --> https://github.com/kbaggen/Sound-Bubble-Logger-and-Temperature-controller-for-TILT/releases/):
* 0xe000  boot_app0.bin 
* 0x1000  bootloader_qio_80m.bin 
* 0x10000 SBL4TILT_TempControl_v1.0.ino.bin 
* 0x8000  SBL4TILT_TempControl_v1.0.ino.partitions.bin 

USe "Flash Download Tools (ESP8266 & ESP32 & ESP32-S2)" https://www.espressif.com/en/support/download/other-tools

### Operate/setup
The Bubble Logger got a captive portal mode and hence you log on just as it was a Wifi access point. If the longin page do not autmatically come up, go to: 192.168.4.1

It will for 60 seconds go into this “Captive Portal” at power on (or after any faliours as lost connection or lost power) where all setting can be done (E.g hence, set Brew Name, SSID+Password, Brew Size, start Temperature, (License, currently not needed), TILT Offset temperature+gravity and Brewfather URL/Ubidots Token/Brewersfriend URL). It light blue when in “Captive Portal” mode (and also blink blue when sending/treating data including dectection of a bubble). 

Hence,
* Connect till the “Bubble Logger CONNECT” SoftAP though either you mobile phone or laptop by Wifi.  After connect till access point the Bubble-logger url for the configuration page is: 192.168.4.1 
* Set the various parameters accordingly (Currently, you need to set SSID+Password every time you enter “Captive Portal” mode, so you need to set SSID+password even you just changed one parameter. A bug we are working on).
* If you later on wish to change for instance temperature, pull power for 2 sec, and go into portal mode agian and change the temperature (remember to set SSID+password agian too).

![alt text](https://github.com/kbaggen/Sound-Bubble-Logger-and-Temperature-controller-for-TILT/blob/master/pic/SBL4TILT_gui.png)

Secondly, the Bubble-Logger also got a webserver you can follow all data on during brewing, to access this on you need to find the IP either in you routning table or some Network sniffer program. The Webserver is on: 192.168.1.xxx:8080 (hence please notice the port is 8080).

You will ofcouse need a Ubidots STEM account and hence the TOKEN (see under API cridentials), and for Brewfather you need to enable "Custum Stream" and inset this asscociated URL into Bubble-logger. I am not good a brewersfriend, but this is similary done as Brewfather.

### What data is send?
The following data is send til Brewfather and Brewersfriend (please notice I am not good at Brwersfriend and hence might be issues/error):
* "Blop pr. min" is send as: BPM
* "Sum BLOPS(pt)/L" is send as: Pressure (PSI)
* "Gravity" is send as: Gravity (G)
* "Temeprature" is send as: Temperature (C)
* "TILT colour" is send as a comment and can be seen under devices.

(Please notice I do not and do not intend to support other than metic numbers. 3rd worlds countries has to add up and follow the scentific rules.)

For Ubidots the above is also send, but also the power-state of relay is send, where the following coding is used.
* 0 = Relay awaiting.
* 1 = Relay cooling.
* 2 = Relay heating.
* 3 = Relay turned of as something wrong ~ No TILT?
* 4 = Relay tirned of as SetTemp is outside supported range of 2-60´C.

### Relay and warning = beta software
This software is in beta and even I have included all the safty I can think off please treat it carefully. The Relay do turn off if no TILT is found, and the heating part of the relay do turn off before each data treatment to secure the heating is off if it goes down. The Bubble Logger do not support temperatures below 2´C or over 60´C and will turn relay of if set outside this range. If the wifi is unstable, and hence if the logger lose wifi it will restart to secure a new connection, hence, there is build in a behavoir of restarting. If you experiance any melt-down where the logger get stuck in either heating or cooling mode, please, let me know!

### Questions/contact?

Facebook group: https://www.facebook.com/groups/2176394599141882

## Buliding sensor and calibration
The digital Sound Sensor Detecting Module LM393 needs to be calibrated to a degree where it is responsive, but where we also can “work” besides make some noise.


### Building Sound sensor with “condom” and placement in airlock.
(Fitting the Condom – Water Balloon on the LM393 – fitting in Airlock – Alginment)
The LM393 need a moisture protection, and this is done by a small water balloon, and it should be rather tight around the noose, but still loose as below pictures shows. It needs to sit tight in the airlock making an seal to restrict any water from vaporization. To allow the pressure to equalize a small hole needs to be drilled. Align it so the mirc is place over the direct hole in the airlock, so the sesor get the direct sound “blup”.
As the sensor got some shapes edges there will flence the ballon and secondly as the mirc rather easily can break off, try steady the sensor by some tape as first picture shows!
![alt text](https://github.com/kbaggen/Sound-Bubble-Logger-and-Temperature-controller-for-TILT/blob/master/pic/buidling%20sensor.png)

Notice the small hole in airlock. This helps fillling and clean the airlock if you choose not to ever remove the s-airlock (is the case of a blow-out system).

Besides the water amount of 4-4,5ml and the use of a calibrated censor the foremost important factor is the alignment of the probe, and it need to be pressed all the way down in the s-airlock and aligned directly over the tube-hole and hence get the direct release of C02 sound/pressure-burst. If not fitted precisely you loose BPM and hence the rG estimate goes wrong if you choose to make use of the polynomial approach as second opion of SG estrimation.

### Calibration.
Make a brew and Put on the “condom” (small water balloon) on the LM393, se below picture! When the BPM is around 15-30 (in avenge over 20-30min) by hear and see count adjust the potentiometer of the sensor till it reflect this by the logger (simply turn the potentiometer down until it stops lighten green and then fine adjust until you get the same BPM as hear and see count) . This is best done at a pressure around 1010-1018 hPa as the bubble rate is easy to detect by hear and see count. This give you a calibrated LM393 ready for your next brew (if you do the calibration very early in the fermentation and a large brew it should also be useful during the brew calibration is done upon).


During the next 1-2 brews you can fine tune the potentiometer of the sound sensor so it reflects the BPM by hear and see. In all, during 3 brews you should hold a calibrated sensor there can give a precision of -/+ 3 gravity units if you make use of polynomail approach.

This give a high resolution sensor there miss a few and also post some double bubbles, but this is fine as long the avenge BPM do reflect you hear and see count. calibration should give you between 50 and up till 100-150 SBM at high krauzen depending on temperature/yeast/brew size (I brew in 14-25L amounts), etc! This setting is prone to high sounds, but light talking, music, drier and washing machine is ok to have nearby!

To be able to compare from brew to brew of BPM and hence make use of polynomial you should try to hold as many variable the same, e.g. same sensor from brew to brew and foremost have same amount of water in airlock (+ same kind of S-airlock). I use 4-4,5 ml. Secondly, ensure the alignment of the probe is the same from brew to brew. Consider to have a blow-out setup where sensor never leaves the S-airlock.
 
## Additionally/ experimental –  Estimate of “reduction in gravity” (rG) - PLAATO emulation
The software can be used to give an indicative rG (reduction in gravity) estimate base on the use of an  S-airlock and a “hear and see” calibrated /aligned sensor is used with a precise amount of water (4-4,5 ml)! This will only function for complete airtight fermenters and even so is only indicative and need further calculation by the user.

What we measure is as said the Blops pr. Min coming when CO2 is released, e.g. BPM, and if we look at the chemistry behind the metabolisms of fermentation of sugar by yeast cells, we see one part Alcohol generate two part CO2. Hence, CO2 is a direct measurement of the alcohol production. The key issue is to measurement this gas accurate and precise. Hence, the need of sealed airtight tanks.

               C6H12O6    ====>   2(CH3CH2OH) + 2(CO2) (+Energy)
               
                Sugar     ====>    Alcohol    +   Carbon dioxide gas (+Energy)

Hence, by knowing the BPM and brew size (L) and other involving constants we can plot a curve of Sum BLOPS/L vs. the reduction in Gravity and generate a model or polynomial for the alcohol production based on our initial measurement of  Blops pr. Min (BPM), se more below.

### Atmospheric pressure + Temperature and Blops pr. Minute – Indirectly impacting the bubbling rate
The Atmospheric pressure do impact on the amount of bubbles in the sense more bubbles can be seen at very low pressure! The assumption is the bubbles is of lower size, and hence the release of CO2 is not higher, we just see more tiny bubbles so to speak, and/or the density of gasses in each bubbles is less.
The temperature also impacts on the activity of gasses, and hence at lower temperature the molecules is not moving as fast and therefore the bubbles rate is lower at lager temperature of 10`C vs. an ale of 20´C.
The “Bubble Logger” software contains complex build in models to account for change of pressure and temperature and reports the “Sum BLOPS(pt)/L” adjusted for the pressure and temperature impact. 

### Estimation of SG by polynomial approach
If your are using an airtight tank and an S-shaped airlock with 4-4,5 ml water and a “hear and see” calibrated/aligned sensor you can to some degree predict the SG by the BPM (e.g. “Sum BLOPS(pt)/L”). It will only be an estimate and sometimes the SG will be way off.
The SG is calculated by we measure the BPM over time and this is re-calculated in regards of “Sum BLOPS(pt)/L” by taken the current pressure, temperature and brew size into account (L), and hence this is used by the polynomial to calculate the rG though a first or second degree polynomial.

![alt text](https://github.com/kbaggen/Sound-Bubble-Logger-and-Temperature-controller-for-TILT/blob/master/pic/SBL4TILT_graph.png)

Looking at below data of the 18 brews done and plotting the reported rG vs. Sum BPMpt/L at the time the brew reached FG, we can model a first or second degree polynomial to fits the data and these polynomials can be used to model the reduction in gravity, rG. Hence, if you repeatedly take notice of the Sum BPMpt/L when FG is reached over 3-5 brews, you should be able to develop a polynomial/decision tool to foreseen the rG by the Bubble-Logger for your equipment.

![alt text](https://github.com/kbaggen/Sound-Bubble-Logger-and-Temperature-controller-for-TILT/blob/master/pic/SBL4TILT_overviewDATA.png)

  

Hence, if making use of above data we can generate a indicative rG table for this equipment setup based on the reported “Sum BPMpt/L” (based on “hear and see” calibrated sensor, 4,5 ml water in S-airlock and airtight fermenter):

![alt text](https://github.com/kbaggen/Sound-Bubble-Logger-and-Temperature-controller-for-TILT/blob/master/pic/SBL4TILT_table.png)
 
Meaning the “Sum BPMpt/L” of 5000 should give and reduction in gravity, rG, around 38-39 and error of mean is around 3 SG units (see former table).
This should in theory function for all airtight fermenters/setup if using a calibrated sensor to “hear and sound” and having a S-shaped airlock with 4-4.5ml water. Every user will need to make there own polynomial based on above process.

Please notice the key or purpose of the Bubble-Logger here is not the rG nor gravity estrimatio, but when the BPM or Sum BLOPS(pt)/L starts to flatten, and hence, when we should start to consider make a hydrometer reading.  All users of both Tilt, Ispindel and PLAATO in the end have to make a hydrometer reading, hence, the game here is to give the user the data to decide when it is time to do so, besides, giving the user data on when yeast activity is falling in regards of decision for dry hopping, temperature changes, cool crash, etc.!   

So the user calculated polynomial and hence rG can be close to real life if a keen eye on airtight tanks, calibration and precise amount of water in airlock, but it cannot stand alone and in this sense a TILT or Ispindel is considered more precise. We believe the activity measurement of the Bubble Logger joint together with TILT gravity data is the best approach the homebrewer can do. 

### Important “take on messeges” if you wanna look into rG
*	One calibrated Sensor = One Airlock.
*	Sensor should be calibrated till “hear and see” matching count.
*	4-4,5ml in S-Airlock.
*	Alignment of sensor should be the same. Consider let the sensor stay in airlock, and clean by alkaline solution, acid and StarSan accordingly.
*	Use a fermenter there is airtight. Be a “Leak Hunter”!
*	Take notice of your amounts in Liters and set this in Bubble-Logger setup.
*	Recalculate the polynomial to your equipment. 
*	Make use of slow and controlled fermentation and/or good headspace.
*	Or, use a blow-out system.
*	Steady WiFi is needed for the logger to obtain data it needs for calculation (e.g. surrounding pressure). 

