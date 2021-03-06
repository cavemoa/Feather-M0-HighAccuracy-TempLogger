# Feather-M0-HighAccuracy-TempLogger

### [Wiki Pages](https://github.com/cavemoa/Feather-M0-HighAccuracy-TempLogger/wiki) have code descriptions and more details

### General Idea & Concept
This will be a series of files developing the ideas and concepts around getting the maximum temperature accuracy and resolution from an Adafruit Feather M0. These great little board have some real grunt and after reading the amazing posts over at the [CavePearl project](https://thecavepearlproject.org/) I thought I would follow in Edward Mallon's footsteps. If you have never read his blog posts and you are even faintly interested in logging with Arduino's then you should. What he has achieved is amazing and his concept of using the cheapest parts and arduino clones in such harsh environments is amazing. 

Recently he wrote two posts, one on ["How to read thermistors"](https://thecavepearlproject.org/2016/06/09/better-thermistor-readings-with-an-arduino-series-resistors-aref/) and one on [oversampling the arduino ADC](https://thecavepearlproject.org/2017/02/27/enhancing-arduinos-adc-resolution-by-dithering-oversampling/). The punch line is he is able to calibrate and read a generic Therimstor  delivering an effective resolution of ~0.0028°C with a 10bit Arduino cloneboard. Thats pretty amazing and his articles cover the trials/tribulations and learnings on trying to do this. The articles take some reading, not becasuse its complex but because there are a few pieces to the puzzle and also there are few side references you need to read to really understand things. Basically you need to go on the journey with him.
The end result though is that you realize with almost any thermistor, and some carefully chosen components you can read them straight off the bat with the ADC to ~0.01°C resolution. The trick is to pick the temperature range you want (e.g. 0-25°C or -5-50) and then carefully fit the thermistor voltage generated from Vcc - 0v rail to rail against a more restricted Vref range on the ADC. Say you use the internal 1.1V Aref. That way you maximize the value of each of your valuable 2^10 / 1024 bits. 
> Note: This isn't a silver bullet, its a "how to make a silver bullet". You have to optimize your actual range against your actual Thermistor though he has recipes to get you going.

On the journey you have to think serious about the self heating of the thermistors by the the volateg you put through them to read them. You also dive headlong down a  series of rabbit holes about what voltages you actually know (i.e. is Aref 1.1V or 1.095V on your arduino?). Some of those rabbit holes are deep with many passages I warn you!!
So once you have maximized your ADC range the next trick is to oversample to get ever higher resolution, but to do that you need to put in some random noise. Edwards block explains that better than I can. But again with only a few components you can then make the leap from 10bit on a 10bit ADC to 14bit on a 10bit ADC!!! Granted you need to take a couple of samples

> From the cave pearl post: "you need to read the ADC four to the power of n times.  Generally you have to add three extra bits (43= 128 samples) to see approximately an order of magnitude improvement in your real world resolution. With thermistor dividers, you typically get about 0.1°C from the default ADC readings, and 128 samples bumps that to 0.012°C.  Taking (46= 4096) samples would bump that up to ~0.0015°C which, as the saying goes, is good enough for government work…"

That works for me!

So Edward uses the 328 Arduino clones, my weapon of choice is the [Adafruit Feather M0](https://www.adafruit.com/product/2796) usually in the logger version. So my plan is to follow edwards journey but using the [Adafruit Feather M0](https://www.adafruit.com/product/2796) and a readily available 10K Thermistor (beta: 3950) from [Sparkfun](https://www.sparkfun.com/products/250).

> Can I get a $0.75 Thermistor and a $19.95 Feather to a resolution of 1 milli kelvin / 0.001°C I guess we will see !

Now be careful, resolution and accuracy are very different so I'm also going to see what accuracy I can actually reproduce repeatedly. Thats not code & maths its calibration so I guess we will see but that will more likely be 0.1 °C using an icebath. I think my challenge here is trying to get multiple calibration points at sensible temperature range.
My personal interest is also in caves so I need to measure 6-12°C range realistically. I'm afraid if you are looking for -20-100°C thats journey you will be going on alone !

