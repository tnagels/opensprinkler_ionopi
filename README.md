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

 - **This is a good time to change your RPI password, it's the first item in the list for a reason!**
 - Second thing to check is your timezone. So go into "Localisation options" and set the timezone to your location.
 - GO into "Network Options" and set your hostname to whatever you want. If you need to set a fixed IP or connect to a wireless network, you can do this here.
 - Then go to “Interfacing Options”, and enable follwing interfaces
	 5. 
 - List item

“SPI” and select “yes” to enable the SPI interface. Also enable "SSH" for remote access (This is why you must change your password!). Finally enable "I2C", we will need it later.
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
    
Now the RPI will reboot and the RTC should be active. Assuming you are connected to the Internet and your Pi was able to reach a public NTP (Network Time Protocol) server, you should see the current date and time using the `date` command: 
Also check the date and time stored in the hardware clock: 

    $ sudo hwclock -r 

If the returned date and time is not correct, or “hwclock” returns an error, use the “-w” option to set the hardware clock to the current time: 

    $ sudo hwclock -w 

Then recheck the time stored in the hardware clock to ensure it matches. Linux may have failed to automatically update the hardware clock after the last reboot if its internal registers contained invalid values.

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTI2NjMzNzQxLC03NDcyMzEwNTQsLTEyNT
k1ODk1MTEsLTEwNzMzNzA1NjUsLTk0MDI0NDNdfQ==
-->