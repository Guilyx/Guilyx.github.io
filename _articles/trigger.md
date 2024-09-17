---
layout: page
title: Trigger system
category: "major"
---

Not everything that a detector measures is actually a signal from an event that we're interested in (like a muon passing a scintillator). One way to filter out the junk is by data analysis, this is called triggerless DAQ (data acquisition). This entails noting down all datapoints you were ever able to measure. The data analysis method is actually too overpowered in this case, because the bulk of the junk data can be eliminated with something a lot simpler, like a relay logic. This is especially nice because we aren't wasting storage space on useless information. The relay logic or relay system can also be referred to as the umbrella term trigger logic (trigger system). This method, also known as synchronous DAQ is the main competitor of the triggerless DAQ.

<img src="/img/DAQtypes.png" alt="image" width="100%" height="auto">

In the case of a synchronous DAQ, a trigger basically brushes through the signals like a haircomb: it organizes the signals into individual strands of hair (events), while brushing out all of the junk. Our data analysis software ROOT is built for handling data in events. 
<img src="/img/ROOT.png" alt="image" width="100%" height="auto">

On this picture, each branch is data, which we get from a detector or a single output of some detector. The entries in multiple branches were concurrent if they are in the same entry.

For example, a trigger system that I should've used previously, but didn't, is recording  voltage signals only if they exceed some threshold voltage. How I did this was by requiring the Arduino to measure the voltage at every moment, but note it down only if it exceeds a threshold. This is not a trigger system, because the signal wasn't filtered BEFORE reaching the detector (Arduino's ADC). The trigger system that should've been attached before the ADC is given on the following picture.
<img src="/docs/assets/easy_trigger.png" alt="image" width="100%" height="auto">

The discriminator works just like a comparator. It's output is used as a reference point for when to take measurements. Thanks to the delay component, the comparator's output transition (from 'high' to 'low' or vice versa) occurs precisely when the signal reaches the ADC. The second transition, which signifies the end of the measurement, occurs as the tail end of the signal passes the ADC.
<img src="/images/discriminator.png" alt="image" width="100%" height="auto">

Here you see the output of a dicriminator and its appearance on a NIM.

In synchronous DAQ, events are never perfectly synchronous. Delays from wires of different lengths can be on the order of 100ns. Additionally, the time difference caused by a particle hitting two detectors is 3.3ns per meter. In our 6m testing area, this results in a delay of up to 20ns between the detectors. Both factors cause signals to arrive at different times.

If we need to set three signals in coincidence, we should first group the two signals that arrive closest to each other. Then, we set the obtained coincidence signal into a second coincidence with the third signal. This helps us, since the length of a coincidence signal can be manually set. The 1st and 2nd signal only dictate the start of their coincidence signal, but not the end. So even if the 1st and 2nd signal decay before the arrivel of the 3rd, if we set our coincidence signal long enough, then there will be a moment in which we have both the coincidence of signal 1 & 2 and signal 3. 
<img src="/img/coincidence_better.png" alt="image" width="100%" height="auto">


Trigger systems can become very complicated, for example an extra layer of complexity is added if we take two previously explained trigger systems and combine the outputs of our two discriminators with an AND gate. This results in the coincidence method: if the voltage is above a treshold at both detectors simultaneously, then we know to take measurements. 
There is a short yet not infinitesimal time frame in which the signal needs to pass through the detector and other components until it gets stored on the disk. On the image, this time frame is 1ms. We cannot have a new trigger go off during that same time, since the processing of the previous one has still not yet finished, so we need to ignore all incoming signal for that time period. This is done by a busy logic.
<img src="/docs/assets/my_trigger.png" alt="image" width="100%" height="auto">

A busy logic is very intuitive: if your system says it's 'BUSY', then it's unaccepting of new signals because it has other stuff to do. It is ready to take on a new signal once it goes into 'NOT BUSY' mode.
The most comprehensive [presentation](https://indico.cern.ch/event/1337180/contributions/5629322/attachments/2880440/5046367/isotdaq24.Negri.DaqIntro.pdf) on this topic.

Do signals usually come in bunches or are they more-or-less equally spaced? Well, as it turns out, there is a mathematical reason for them to come in bunches. For events that are both stochastic (random) and memoryless, the probability of an event happening is always highest at the current moment. <img src="/img/time_between.png" alt="image" width="100%" height="auto">



