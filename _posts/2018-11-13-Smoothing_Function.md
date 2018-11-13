---
title: "Smoothing function for arduino."
date: 2018-11-13
tags: [lab tool, arduino]
categories: [code, arduino]
header:
  image: "images/header_image2.png"
excerpt: "This little code snippet is for smoothing arduino input readings."
---
<img src="{{ site.url }}{{site.baseurl }}/images/ArduinoCommunityLogo.png" alt="ArduinoCommunityLogo" width="50"/>

The arduino input pins are not very clean sensor inputs. I used this code to take an array of sensor readings, remove the top/bottom 15% outliers, and take an average of the remainder. This doesn't work as well as proper hardware grounding/isolation but it was surprisingly effective.

[Download](https://github.com/scotttmoen/code)
