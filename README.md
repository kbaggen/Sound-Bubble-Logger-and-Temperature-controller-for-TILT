# Sound-Bubble-Logger-for-TILT

Welcome and thanks for downloading and building a “Sound Bubble-Logger for Tilt”!
The Sound Bubble-Logger is an multi function Arduino fermentation logger there monitor your yeast activity by motioning the CO2 release though blops pr. minute (BPM) and can display this information into Ubidots, Brewersfriend or Brewfather.  
It also works as a TILT repeater and hence can include your Tilt data (gravity + wort temperature) into the “payload” sent till the cloud services and hence making this part easier. 
Knowing the yeast activity though CO2 bubble measurements over time including the start of decline of acivity, we can better foresee when the SG is close to FG, and better make decision on dry-hopping, temperature changes, etc.! Combining the BPM, Sum BLOPS data, and TILT data giving the homebrewer the best data we can get for decision.
Hence, Sound Bubble Logger For TILT gives you:
1.	Reports the activity of the yeast as CO2 escape the fermenter by BPM.
2.	Give you a pressure and temperature corrected sum of Blops, e.g. “Sum BLOPS(pt)/L”, there likewise is a measurement of the fermentation progression. As it is corrected for pressure and temperature the “Sum BLOPS(pt)/L” can be used to compare from brew to brew and hence give an additionally measurement of fermentation and its progression.
3.	“Tilt repeater”, and hence repeats the SG and in wort temperature.
4.	Take care of sending all data.
5.	Additionally/experimental -  “PLAATO EMULATION”: The software can be used to give an indicative rG (reduction in gravity) from the “Sum BLOPS (pt)/L” and as such emulate what PLAATO is doing. This needs complete airtight fermenters to be trustworthy and even so is only indicative and need calculation of polynomial by the user.

Additionally/ experimental –  Estimate of “reduction in gravity” (rG)
The software can be used to give an indicative rG (reduction in gravity) estimate base on the use of an  S-airlock and a “hear and see” calibrated /aligned sensor is used with a precise amount of water (4-4,5 ml)! This will only function for complete airtight fermenters and even so is only indicative and need further calculation by the user.
What we measure is as said the Blops pr. Min coming when CO2 is released, e.g. BPM, and if we look at the chemistry behind the metabolisms of fermentation of sugar by yeast cells, we see one part Alcohol generate two part CO2. Hence, CO2 is a direct measurement of the alcohol production. The key issue is to measurement this gas accurate and precise. Hence, the need of sealed airtight tanks.
               C6H12O6    ====>   2(CH3CH2OH) + 2(CO2) (+Energy)
                Sugar     ====>    Alcohol    +   Carbon dioxide gas (+Energy)
Hence, by knowing the BPM and brew size (L) and other involving constants we can plot a curve of Sum BLOPS/L vs. the reduction in Gravity and generate a model or polynomial for the alcohol production based on our initial measurement of  Blops pr. Min (BPM), se more below.
Atmospheric pressure + Temperature and Blops pr. Minute – Indirectly impacting the bubbling rate
The Atmospheric pressure do impact on the amount of bubbles in the sense more bubbles can be seen at very low pressure! The assumption is the bubbles is of lower size, and hence the release of CO2 is not higher, we just see more tiny bubbles so to speak, and/or the density of gasses in each bubbles is less.
The temperature also impacts on the activity of gasses, and hence at lower temperature the molecules is not moving as fast and therefore the bubbles rate is lower at lager temperature of 10`C vs. an ale of 20´C.
The “Bubble Logger” software contains complex build in models to account for change of pressure and temperature and reports the “Sum BLOPS(pt)/L” adjusted for the pressure and temperature impact. 
Estimation of SG by polynomial approach
If your are using an airtight tank and an S-shaped airlock with 4-4,5 ml water and a “hear and see” calibrated/aligned sensor you can to some degree predict the SG by the BPM (e.g. “Sum BLOPS(pt)/L”). It will only be an estimate and sometimes the SG will be way off.
The SG is calculated by we measure the BPM over time and this is re-calculated in regards of “Sum BLOPS(pt)/L” by taken the current pressure, temperature and brew size into account (L), and hence this is used by the polynomial to calculate the rG though a first or second degree polynomial.
Looking at below data of the 18 brews done and plotting the reported rG vs. Sum BPMpt/L at the time the brew reached FG, we can model a first or second degree polynomial to fits the data and these polynomials can be used to model the reduction in gravity, rG. Hence, if you repeatedly take notice of the Sum BPMpt/L when FG is reached over 3-5 brews, you should be able to develop a polynomial/decision tool to foreseen the rG by the Bubble-Logger for your equipment.
 

 

Hence, if making use of above data we can generate a indicative rG table for this equipment setup based on the reported “Sum BPMpt/L” (based on “hear and see” calibrated sensor, 4,5 ml water in S-airlock and airtight fermenter):
 
Meaning the “Sum BPMpt/L” of 5000 should give and reduction in gravity, rG, around 38-39 and error of mean is around 3 SG units (see former table).
This should in theory function for all airtight fermenters/setup if using a calibrated sensor to “hear and sound” and having a S-shaped airlock with 4-4.5ml water. Every user will need to make there own polynomial based on above process.
Please notice the key or purpose of the Bubble-Logger here is not the rG nor gravity estrimatio, but when the BPM or Sum BLOPS(pt)/L starts to flatten, and hence, when we should start to consider make a hydrometer reading.  All users of both Tilt, Ispindel and PLAATO in the end have to make a hydrometer reading, hence, the game here is to give the user the data to decide when it is time to do so, besides, giving the user data on when yeast activity is falling in regards of decision for dry hopping, temperature changes, cool crash, etc.!   
So the user calculated polynomial and hence rG can be close to real life if a keen eye on airtight tanks, calibration and precise amount of water in airlock, but it cannot stand alone and in this sense a TILT or Ispindel is considered more precise. We believe the activity measurement of the Bubble Logger joint together with TILT gravity data is the best approach the homebrewer can do. 
Important “take on messeges” if you wanna look into rG
o	One calibrated Sensor = One Airlock.
o	Sensor should be calibrated till “hear and see” matching count.
o	4-4,5ml in S-Airlock.
o	Alignment of sensor should be the same. Consider let the sensor stay in airlock, and clean by alkaline solution, acid and StarSan accordingly.
o	Use a fermenter there is airtight. Be a “Leak Hunter”!
o	Take notice of your amounts in Liters and set this in Bubble-Logger setup.
o	Recalculate the polynomial to your equipment. 
o	Make use of slow and controlled fermentation and/or good headspace.
o	Or, use a blow-out system.
o	Steady WiFi is needed for the logger to obtain data it needs for calculation (e.g. surrounding pressure). 
