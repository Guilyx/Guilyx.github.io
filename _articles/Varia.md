---
layout: page
category: "minor"
title: Varia
---

CESAR is a software that deals with the beam configuration and also does measurements on a spill level. This means that it can identify the no. of particles in a spill that were counted by xyz detector but it cannot measure anything on a particle level. Hence, in CERN context this is not considered a DAQ (data acquisition system).
TDAQ is a trigger and data acquisition system. This one can do measurements on the particle level.
The no. of particles that you have in the beam is given by two numbers: the event and the trigger rate. Event rate is the one measured by CESAR and this can be taken as the de facto number of particles in a spill. Trigger rate, however, is the number you are actually able to measure with the TDAQ (readout takes some time).
`&` means coincidence, for example `S0&S1&XCET040` is scintillators 0 and 1 set into coincidence with each other and the Cherenkov 40. 
NIM - nuclear instrumentation module 
