# opensprinkler_ionopi
Implementation of OpenSprinkler on an Iono Pi
## Introduction
(To be completed)
## Hardware
You need an [Iono Pi](https://www.sferalabs.cc/product/iono-pi/), the version I used is the one withan RPI 3B+ because I had it lying around. You can also use newer versions, but beware; some of the instructions will probably change for the RPI 4.
The main advantage of the Iono Pi is that it already has "hardened" I/O which makes interfacing it to the sensors and actuators very easy.
## Operating system & boot configuration
I installed the standard Raspbian Lite using the [Raspberry Pi Imager](https://www.raspberrypi.org/downloads/).
Then, on the SD-card edit the /boot/config.txt file and add the following:

    gpio=16,19,13,12,6,5=ip,pn
    dtoverlay=act-led,gpio=7
    dtparam=pwr_led_trigger=heartbeat
The first command is needed for proper operation of the inputs, the second sets the activity led to the front green led of IonoPi. The third changes the function of the activity led to "heartbeat" which gives a clearer indication of how hard your Pi is working.
Put the SD-card in your Pi and boot it for the first time.
##
    

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTM4MTI0MDU3MywtMTI1OTU4OTUxMSwtMT
A3MzM3MDU2NSwtOTQwMjQ0M119
-->