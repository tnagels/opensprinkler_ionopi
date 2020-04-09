# opensprinkler_ionopi
Implementation of OpenSprinkler on an Iono Pi
## Introduction
(To be completed)
## Hardware
You need an [Iono Pi](https://www.sferalabs.cc/product/iono-pi/), the version I used is the one withan RPI 3B+ because I had it lying around. You can also use newer versions, but beware; some of the instructions will probably change for the RPI 4.
The main advantage of the Iono Pi is that it already has "hardened" I/O which makes interfacing it to the sensors and actuators very easy.
## Operating system & configuration
I installed the standard Raspbian Lite using the [Raspberry Pi Imager](https://www.raspberrypi.org/downloads/).
Then, on the SD-card edit the /boot/config.txt file and add the following:

    gpio=16,19,13,12,6,5=ip,pn
    

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEwNzMzNzA1NjUsLTk0MDI0NDNdfQ==
-->