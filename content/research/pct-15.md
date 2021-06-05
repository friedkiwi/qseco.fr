---
title: "PCT-15 Audio decoding"
date: 2021-06-05
draft: false
---

I've just watched the recently released [Techmoan video](https://youtu.be/8_Yz0TT439Q) titled "Sony's forgotten ‘80s Picture Phone". As with all of his videos, I found it quite interesting and he has uploaded the audio files so I could do an attempt at trying to decode it.

I've uploaded a copy of the audio file [Here](PCT-15-AUDIO.wav) for posterity.

# First attempt

My first attempt at trying to decode the audio file resulted in me opening the audio file in Audacity:

![audacity clean](audacity_clean.png)

For those who are familiar with recordings of e.g. Commodore 64 tapes, a few things become immediately obvious:

- the signal starts with a 400ms sync pulse
- the signal proceeds with a 400ms pause
- the data stream is being sent
- the data stream is terminated with silence

Having a closer look at the sync pulse produces an immediately recognisable pattern:

![sync pulse 1](sync_pulse_1.pmg)



