# opensprinkler_ionopi
Implementation of OpenSprinkler on an Iono Pi
## Introduction
Information here was compiled from the [Iono Pi User Guide](https://www.sferalabs.cc/files/ionopi/doc/ionopi-user-guide.pdf) and [instructions found here](https://www.hackster.io/Ryan33/raspberry-pi-web-page-based-sprinkler-controller-00d26f).
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

 1. **This is a good time to change your RPI password, it's the first item in the list for a reason!**
 2. Second thing to check is your timezone. So go into "Localisation options" and set the timezone to your location.
 3. GO into "Network Options" and set your hostname to whatever you want. If you need to set a fixed IP or connect to a wireless network, you can do this here.
 4. Then go to “Interfacing Options”, and enable follwing interfaces
	- "SSH" for remote access; this is why you must change your password!
	- “SPI” for the analog inputs.
	- "I2C", we will need it later for the RTC.
	- "1-Wire" if you need it, or be sure it is disabled if you want to use TTL1 for other purposes. 
5. Exit the tool and choose to reboot your RPI when prompted.

Run the following commands to download and install the Iono Pi utility: 

    $ sudo apt-get update
    $ sudo apt-get install git-core 
    $ cd /usr/local
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
## Installing OpenSprinkler
-  `sudo su`  
    so you don’t have to `sudo` for every command.
-   Change directory to /usr/local  

    `cd /usr/local`

-   Clone the OpenSprinkler firmware repository.  
    This will create a directory in /usr/local called OpenSprinkler.  
    `git clone https://github.com/OpenSprinkler/OpenSprinklerGen2 OpenSprinkler`
-   Go into the OpenSprinkler directory and build the firmware.  
    `cd OpenSprinkler  
     csudo ./build.sh`
-   The build script will ask if you want to run the software on startup, answer yes. If it compiles ok, you should have an executable `/usr/local/OpenSprinkler/OpenSprinkler` ready to go. Go ahead and run the executable to test it. Press control-C to exit.
-   Reboot your Pi and OpenSprinkler should be started automatically. You can test with  
    `pgrep OpenSprinkler  
    `If it returns the process id number, then OpenSprinkler was started automatically and is running. If it returns nothing, then something went wrong; check the previous steps.

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEwNDQ1NTc0NzQsMTkyNDM4NDI0Nyw1ND
U4MTEyNDcsMTI1OTE2MDA4NCwxODc4MzQyOTcwLC03NDcyMzEw
NTQsLTEyNTk1ODk1MTEsLTEwNzMzNzA1NjUsLTk0MDI0NDNdfQ
==
-->