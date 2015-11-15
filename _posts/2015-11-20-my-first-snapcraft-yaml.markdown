---
layout: post
title:  "My first snapcraft.yaml"
date:   2015-11-20 10:00:00
categories: snappy raspi snapcraft
image: building-blocks.jpg
---

Here’s the snapcraft.yaml file for my demo project:

{% highlight yaml %}
name: minecraft-minimu9
version: 1
vendor: Alejandro J. Cura <alecu@protocultura.net>
summary: webgl + websockets + MinIMU-9 demo
description: This demo gets data from a MinIMU-9 via websockets, and uses that data to interact with a three.js scene
icon: icon.png
services:
    serial-port-json-server:
        start: bin/serial-port-json-server
parts:
  spjs:
    plugin: go
    source: git://github.com/alecu/serial-port-json-server
    filesets:
      spjs:
        - bin/serial-port-json-server
    snap:
      - $spjs
  glue:
    plugin: copy
    files:
      html: html
{% endhighlight %}

There’s very little boilerplate in the form of summary, description, icon, vendor, and after that there are the interesting sections like “parts” and “services”.
I defined two parts: “spjs” and “glue”.
“spjs” is the serial-port-json-server, a go project that provides a way for a browser to communicate via websockets with a serial port on the server. I’ve forked it with a very small change to make it also serve html files from a folder on the server. Snapcraft will fetch this other repo for serial-port-json-server, will build this as a binary, and add it to the fileset of files to put inside the snap package.
“glue” uses the “copy” snapcraft plugin to copy my html folder into the snap. This folder contains a copy of the “minecraft” demo from three.js, slightly adapted so it no longer accepts input from the mouse and keyboard, and instead it uses a websocket connection to the server, to read the serial port via spjs, and get the serial data from the lines outputted by the MinIMU-9 attached to the Arduino attached to the raspi. Then it processes the pitch-yaw-roll info in the serial lines, and adjusts the 3d camera accordingly, and uses the data from the button pushes in order to move or stay still.
Finally, the “services” section specifies that the serial-port-json-server should be managed by snappy as a system service, so it gets started on boot, and restarted if it ever crashes.

----
This post is part 5 in a series of posts on how to get started writting IoT apps for the snappy Ubuntu Core.

[core-download]:    http://cdimage.ubuntu.com/ubuntu-snappy/15.04/stable
[raspi-write-sd]:   https://www.raspberrypi.org/documentation/installation/installing-images/
