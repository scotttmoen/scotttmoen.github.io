---
title: "Stepper motor actuated datalogger with temperature/humidity sensor."
date: 2018-11-13
tags: [lab tool, arduino]
categories: [code, arduino]
header:
  image: "images/header_image2.png"
excerpt: "I created a lab instrument that along with recording an analog input, recorded time/date/temp/humidity."
---
<img src="{{ site.url }}{{site.baseurl }}/images/ArduinoCommunityLogo.png" alt=" ArduinoCommunityLogo" width="50"/>

This datalogger records the input from an Adafruit_TCS34725 color sensor, the temp/humidity from a DHT-compatible thermometer, and the time/date from RTC DS1307 clock. The data is recorded at given intervals and then written to an SD card and sent to the thingspeak website for online viewing. I was able to easily power the device for a couple of days on battery, taking recordings every couple of hours.

[Download](https://github.com/scotttmoen/code)
