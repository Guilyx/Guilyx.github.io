---
title: Trigger system
---

Not everything that a detector measures is actually a signal from an event that we're interested in (like a muon passing a scintillator). One way to filter out the junk is by data analysis. This entails noting down all datapoints you were ever able to measure. The data analysis method is actually too overpowered in this case, because the bulk of the junk data can be eliminated with something a lot simpler, like a relay logic. This is especially nice because we aren't wasting storage space on useless information. The relay logic/system can also be referred to as the umbrella term trigger logic/system.
For example, a trigger system that I should've used previously, but didn't, is recording  voltage signals only if they exceed some threshold voltage. How I did this was by requiring the Arduino to measure the voltage at every moment, but note it down only if it exceeds a threshold. This is not a trigger system, because the signal wasn't filtered BEFORE reaching the detector (Arduino's ADC). The trigger system that should've been attached before the ADC is given on the following picture.
![Easy trigger](/docs/assets/easy_trigger.png)
The discriminator works just like a comparator. It's output is used as a reference point for when to take measurements. Thanks to the delay component, the comparator's output transition (from 'high' to 'low' or vice versa) occurs precisely when the signal reaches the ADC. The second transition, which signifies the end of the measurement, occurs as the tail end of the signal passes the ADC.
![Discriminator](/docs/assets/discriminator.png)
Here you see the output of a dicriminator and its appearance on a NIM.

Trigger systems can become very complicated, for example an extra layer of complexity is added if we take two previously explained trigger systems and combine the outputs of our two discriminators with an AND gate. This results in the coincidence method: if the voltage is above a treshold at both detectors simultaneously, then we know to take measurements. 
There is a short yet not infinitesimal time frame in which the signal needs to pass through the detector and other components until it gets stored on the disk. On the image, this time frame is 1ms. We cannot have a new trigger go off during that same time, since the processing of the previous one has still not yet finished, so we need to ignore all incoming signal for that time period. This is done by a busy logic.
![Our trigger](//docs/assets/my_trigger.png) 
A busy logic is very intuitive: if your system says it's 'BUSY', then it's unaccepting of new signals because it has other stuff to do. It is ready to take on a new signal once it goes into 'NOT BUSY' mode.
The most comprehensive [presentation](https://indico.cern.ch/event/1337180/contributions/5629322/attachments/2880440/5046367/isotdaq24.Negri.DaqIntro.pdf) on this topic.
