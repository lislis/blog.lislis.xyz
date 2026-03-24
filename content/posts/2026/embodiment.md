+++
title = "Embodiment and Sound"
date = 2026-02-25
draft = false
tags = ['sensors', 'wearables', 'performance', 'OSC', 'ESP32']
+++

## Context

My study program ([Creative Technologies MA](https://www.filmuniversitaet.de/studium/studienangebot/masterstudiengaenge/creative-technologies)) collaborated with the [acting program](https://www.filmuniversitaet.de/studium/studienangebot/bachelorstudiengaenge/schauspiel) on their final performance for their movement and choreography module.

The idea was that through their bodies and movement they manipulate the sound (Do you get the title now?)

## Tech setup

This was a great opportunity to play with different approaches "track" bodies and movement and the different students and student assistants involved were given free choice! Hurray!

The general pipeline is that all trackers, sensors, etc, send OSC to TouchDesigner, where pre-processing of the incoming data happens, and then mappings out to Ableton. In Ableton we have different presets for different scenes of the performance, and different parameters are being changed automatically by the TouchDesinger data.

We also reserved some manual, live-performance moments for us as well. Also as backups if things fail during the performance.

We chose body tracking with the kinect, a wearable DIY-breathing-sensor and gyro-trackers from the xsense motion tracking-suit.

## Wearable breathing

My approach was building a wearable-breathing sensor inspired by this [knitted stretch sensor from kobakant](https://www.kobakant.at/DIY/?p=1762) and [this random Youtube video on breath sensing](https://www.youtube.com/watch?v=WDRokF_ZW9A).

My setup needed to be fully autonomous and mobile, as the perfromers would wear this and run around the room.

I choose the ESP32 C3 Supermini (mouthful!) as main board for sending OSC data wirelessly, then a pice of conductive steel-thread, crocheted together with elastic band to act as the stretch sensor.

From a previous project I had a bunch of rechargeable 9V batteries laying around, so I decided to use these as power suppplies, together with mini360 buck-step-down converter to go down to 3.3V for the ESP.

All of the circuitry is then sewed onto an elastic band (with a small puch for all cables and the battery) and can be closed with velcro.

Code wise I read the resistance from the thread and normalize it with a dynamic range and send it out as OSC.

## Weird stuff and lessons learned

### Overheating

Initally I stepped down to 5V and went over the 5V-pin of the ESP. Which produced a lot of excess heat and I think even burned two of my boards.

After some research I stepped down to 3.3V and the corresponsing pin because that doesn't go over the internal regulator. Which I'm still confused about tbh, because incoming 5V should not have to be regulated that much?? Not sure. But this fixed the issue with the overheating.

I also lowered the CPU clock frequency to reduce computing and therefore heat as a second measure. `setCpuFrequencyMhz(160);` or 80 as value, default being 240.

That seemed to work fine and fix the issue.

### Data only sending when touching the board

I noticed that some boards only seemed to be sending out OSC when I touched the board itself. WTF?

After some research turns out that some "cheaply" made Super Minis have issues with their onboard antennas, which can be ~somewhat fixed by reducinng wifi power like so `WiFi.setTxPower(WIFI_POWER_8_5dBm);` immedeately after `Wifi.begin()`. For me it's not a fix 100% of the time, but good enough.

## Outcome

tbd
