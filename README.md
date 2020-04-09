# opensprinkler_ionopi
Implementation of OpenSprinkler on an Iono Pi
## Introduction
(To be completed)
## Hardware
You need an [Iono Pi](https://www.sferalabs.cc/product/iono-pi/), the version I used is the one withan RPI 3B+ because I had it lying around. You can also use newer versions, but beware; some of the instructions will probably change for the RPI 4.

The main advantage of the Iono Pi is that it already has "hardened" I/O which makes interfacing it to the sensors and actuators very easy.

To start, connect the Iono Pi to network, display, keyboard and power.
## Operating system & boot configuration
I installed the standard Raspbian Lite using the [Raspberry Pi Imager](https://www.raspberrypi.org/downloads/).
Then, on the SD-card edit the /boot/config.txt file and add the following:

    gpio=16,19,13,12,6,5=ip,pn
    dtoverlay=act-led,gpio=7
    dtparam=pwr_led_trigger=heartbeat
The first command is needed for proper operation of the inputs, the second sets the activity led to the front green led of IonoPi. The third changes the function of the activity led to "heartbeat" which gives a clearer indication of how hard your Pi is working.
Put the SD-card in your Pi and boot it for the first time.
## Installing Iono Pi utilities
### I/O tools
You should enable the SPI interface to use the Iono analog to digital converter, and 1-Wire support if you need to connect 1-Wire devices. To enable SPI, run the “raspi-config” configuration utility: 

    $ sudo raspi-config

**This is a good time to change your RPI password, it's the first item in the list for a reason!**
Then go to “Interfacing Options”, “SPI” and select “yes” to enable the SPI interface, and enable SSH for remote access (This is why you must change your password!). Also enable "I2C", we will need it later.
Also enable 1-Wire if you need it, or be sure it is disabled if you want to use TTL1 for other purposes. Exit the tool and choose to reboot your RPI when prompted.

Run the following commands to download and install the Iono Pi utility: 

    $ sudo apt-get update
    $ sudo apt-get install git-core 
    $ git clone https://github.com/sfera-labs/iono-pi-c-lib.git 
    $ cd iono-pi-c-lib 
    $ sudo sh build
You can run the `iono` command without oprtions to see what tools are available.
### Real-time clock
To install the “i2c-tools” package: 

    $ sudo apt-get update 
    $ sudo apt-get install i2c-tools 

With these prerequisite installs completed, you should download and run Iono Pi’s RTC installation script: 

    $ cd 
    $ wget http://sferalabs.cc/files/strato/rtc-install 
    $ chmod 755 rtc-install 
    $ sudo ./rtc-install 

If the script completes with no errors, delete the installation script and reboot: 

    $ rm rtc-install 
    $ sudo reboot
Now the RPI will reboot and the RTC should be active. 



<!--stackedit_data:
eyJoaXN0b3J5IjpbLTYxNDQxMTY1NywtNzQ3MjMxMDU0LC0xMj
U5NTg5NTExLC0xMDczMzcwNTY1LC05NDAyNDQzXX0=
-->