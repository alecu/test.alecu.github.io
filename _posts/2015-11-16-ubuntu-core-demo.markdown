---
layout: post
title:  "Playing with Arduino, Raspberry Pi2, Snappy Ubuntu Core, Webgl and Websockets"
date:   2015-11-16 10:00:00
categories: snappy arduino raspi webgl websockets
image: minecraft-world.png
---
This is my demo of how you can use your device running the snappy Ubuntu Core to gather data from an Arduino connected to some specific piece of hardware, and how to use that to update a browser in real time via WebSockets.

tl;dr, here’s the demo video:

<iframe width="853" height="480" src="https://www.youtube.com/embed/MSEjZq_Qp6M?rel=0&amp;showinfo=0" frameborder="0" allowfullscreen> </iframe>

https://goo.gl/photos/ezGiAokLG7HpLyoC9

A few months ago I got the Orange Matchbox that I intended to use for my work on the snappy scope, but almost immediately I got moved to some more urgent tasks. Kyle took over that, and ended up producing this awesome video.
My orange matchbox sat on my desk accusingly. I started to shop for ideas on how to use it, and learn about snappy and snapcraft on the process. I decided that it would be fun to use the MinIMU-9 module I have on my hackerspace box. The MinIMU-9 module is a wonderful little thing that holds an accelerometer, a gyroscope, and a magnetometer, all easily accessible via I²C.
So I needed to find a way to somehow show these values in a 3d scene. And since Internet-of-Things devices usually need to show the data they collect in a browser, I figured it would be nice to use webgl and websockets.
After some research I ended up deciding that webgl was too low level, and that something sitting on top like three.js would be much easier. And looking at the page with three.js samples, I got immediately drawn to the “minecraft”-like example. There, I got the base for my demo.
I spent some time trying to write a MinIMU-9 driver using the raspi’s I²C gpio pins, but it was too much hassle, a decided instead to reuse as much as possible for this demo. So I put an arduino in the middle, because the arduino code to convert the I²C data into proper pitch, yaw, roll values is already there, and there’s code to connect to a serial port via websockets available too, so I would only need some tuning.
I ended up doing some changes to the three.js minecraft demo so it would connect via websockets to the raspi’s serial port, and adjust the camera in the scene with the 3d values provided by the MinIMU-9. Also a very tiny change in the code in the Arduino so my “move forward” button would be sent together with the 3d data. And finally, I added a bit of code to the serial-port-json-server so it would serve the html and js files of the modified minecraft demo. Here’s the wiring diagram I used:

The other part was figuring out the best way to build all of that into a snap package, and get it into the raspi2. There were some back and forths, but it was mostly painless.
I’ve made a few further posts with some details on some of those steps:

