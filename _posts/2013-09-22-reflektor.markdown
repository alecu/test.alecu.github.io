---
layout: post
title:  "РЕФЛЕКТОР (reflektor) en el Museo del Juguete San Isidro"
date:   2013-09-22 10:00:00
categories: arduino electromechanical games reflektor
image: reflektor-full.jpg
---
This is a video of my first electromechanical game, being presented in the Toy Museum in San Isidro (Buenos Aires):

<iframe width="640" height="360" src="https://www.youtube.com/embed/G4HYuvcE_QI?rel=0&amp;showinfo=0" frameborder="0" allowfullscreen> </iframe>

You need to precisely align some mirrors that will allow you to reflect the laser to the goal, but only after solving some puzzles and traps, and you need to do it before time runs out. It's inspired by a game by Costa Panayi called [Deflektor][deflektor], that I used to play in the eighties on a CZ Spectrum.

Reflektor was made using an Arduino board for the game logic, another Arduino board plus an Adafruit [Wave Shield][wave-shield] for the music, a custom board with 74HC595 and L293D chips to drive some stepper motors taken from old floppy drives and printers, an old gutted radio for the controls and speaker, and some random recycled waste. Oh, and a green laser, a smoke machine and some mirrors. It's all held together by some plywood and a lot of pre-1960's red and green Meccano pieces.

The [schematics and source code][reflektor-github] are available in case you want to build your own. Feel free to contact me with any questions or if you'd like to bring it to your event.

[wave-shield]: https://www.adafruit.com/products/94
[reflektor-github]: https://github.com/clubdejaqueo/reflektor
[deflektor]: https://en.wikipedia.org/wiki/Deflektor
