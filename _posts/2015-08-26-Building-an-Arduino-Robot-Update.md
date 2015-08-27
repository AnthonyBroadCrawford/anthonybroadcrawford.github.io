---
layout: post
title:  "Building an Arduino robot - Update"
date:   2015-08-26 21:11:51
categories: Arduino Robotics Microbot
---

It's been a few months since I purchased the [Arduino][Arduino] [Parallax robot kit][Parallax robot kit].  It's a mix of hardware and software with a "following the bouncing ball"tutorial on
how to bring the robot to life.

The `robot` is simple.  It's a three wheeled single axle chassis with "collision detectors"
mounted on the front to help it change directions when it comes in contact with a
stationary object.  Basically a Rumba without a vacuum, ergo value.

I've always loved robots and electronics as a kid.  Loved it even more in college
when I had to take two years worth of electrical engineering.  However, I never
got the opportunity to get my hands dirty physically building something electronic.  The [Arduino][Arduino] [Parallax robot kit][Parallax robot kit] has scratched that itch.

I'm guessing I'm about 2/3 of the way through the initial build out, but my observations thus far

- I love the "shield" concept from Arduino
- The Parallax BOE (Board of Education) shield is awesome
- The "pseudo" [C code][Arduino C] from [Arduino][Arduino] isn't bad, but it's not great
- My daughters LOVE working on this robot.  They've named her Eve.

I look forward to wrapping this up in the next few weekends and then starting my combat drone from
competitive racing.

# The progress
It's still early in the process, but the chassis, peziohertz speaker, and the software are all functioning.  
The two Servo's are calibrated and running.

![]({{ site.rul }}/assets/images/building-an-arduino-robot-update/arduino-parallax-robot-microbot.jpg)

[Parallax robot kit]:   http://www.adafruit.com/product/749
[Arduino]:              https://www.arduino.cc/
[Arduino C]:            https://www.arduino.cc/en/Reference/HomePage
