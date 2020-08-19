# Sound-Bubble-Logger-and-Temperature-controller-for-TILT


The Bubble-Logger is an Arduino device there monitor your fermentation in regards of yeast activity though motioning CO2 blops pr. minute (BPM) and Sum BLOPS corrected for pressure impact on bubling rate and hence giving a compareable measument from brew to brew. Furthermore it repreats the TILT data of gravity and temperature and hence display this information into Ubidots, Breersfriend  or Brewfather. It can also control a heating-argent and cooler.

The software can be used to give an indicative rG (reduction in gravity) estimate based on the use of an from brew till brew samewise aligned S-airlock together with the same calibrated sensor is used with a precise amount of water (4-4,5 ml)!

Hence, this project measure/do:

* 1). the activity of the yeast as CO2 escape the fermenter by a digital sound detector.
* 2). Temperature by TILT
* 3). Gravity by TILT
* 4). Send data to the cloud in a easy way
* 5). Estimate of “reduction in gravity” (rG) can be calculated from the Sum BLOPS(pt)/L based on complex model taking pressure and temperature data into account. In this way the Bubble-Logger emualted a PLAATO. 
* 6). A 2-channel Relay to control a heat and cool source based on the temperature reading of the TILT every 2 min, hence, slow-working heating surce must be used.
