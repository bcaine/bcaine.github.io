---
layout: post
title: Visible Light Communication
date: 2015-12-16T21:41:43-04:00
categories: [project]
excerpt: "Transmitting data with LEDs."
---

Visible Light Communication was my Senior Capstone at Northeastern University. The goal was to use commodity hardware to make a complete system to transmit data from one computer to the other using visible light as the communication medium. 

Essentially, we made a simplex (one direction) [LiFi](https://en.wikipedia.org/wiki/Li-Fi) system using hobbyist parts. Coincidentally, the media has spent a lot of time hyping the concept recently, though our results are nowhere near ["100 times faster than wi-fi"](http://www.bbc.com/news/technology-34942685). Our final system sent data at speeds around 100 kb/s due to transister switch rate limitations. If we had a faster switching transmitter design, our embedded system could handle speeds up to about 5 mb/s.

#### The Team ####

* [Ryan](https://www.linkedin.com/in/ryan-nutile-812814100) - Handled the embedded (read: Assembly) codebase that modulated the transmitter circuit and handled the incoming signals from the receiver circuit.
* [Matt](https://www.linkedin.com/in/matt-schroer-707b7654) - Wrote the Desktop GUI to receive and transmit data over USB to our embedded device.
* [Kyle](https://www.linkedin.com/in/kylebradleyece) - Designed and built our transmitter circuit. Designed our PCBs. 
* [James](https://www.linkedin.com/in/james-croci-b463b971) - Designed and built our receiver circuit, including filtering, amplification, converting back to square wave. 
* [Me](http://bencaine.me) - Wrote the host-embedded interface, implemented encoding and decoding algorithms, designed packetization, and managed system for DMA of RAM to share data with the real-time units. 

#### Software System ####

Our system had three software subsystems that all had to work together. 

* Desktop GUI - A .NET Desktop GUI that allowed a user to select a file and transmit it over our system, or receive a file (and if its text or an image, display it). It passes or receives data from the transceiving software via sockets over USB.
* Beaglebone Transceiving Software - A C++ Library running on the linux portion of our [Beaglebone Black](http://beagleboard.org/bone) that is responsible for receiving or sending data over sockets, packetizing (or depacketizing) it, encoding or decoding it, and managing the shared RAM with the Real-time system via Direct Memory Accesss.
* Realtime System - A [PRU Assembly](http://processors.wiki.ti.com/index.php/PRU_Assembly_Instructions) system that runs on both of the Beaglebones two 200MHz programmable realtime units responsible for pulling data from shared memory and modulating it at an exact rate. The receiver had to watch for a preamble that signaled the start of a packet, align itself to sample in the right location, and read in the data. It then passed it back into memory for the transceiver to depacketize etc.

The Beaglebone Transceiving and Realtime system software can be found in [this Github repository](https://github.com/bcaine/VLC-Transceiver). The realtime code is mostly contained in the "asm" folder, and the rest mostly the transceiver code. 