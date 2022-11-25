# **ClusterHat Swarm Server**
Hello, I made this repository for setting up a raspberry server using a ClusterHat, UPS and, Docker Swarm. I am by no means confident in what I write, I'm currently in the process of learning it as well. However, if any of it is able to someone then that makes me happy. 
Hopefully I will be useful to maintain this and keep it up to date.

<img src="images/server.jpg" width="450" height="400"> 

# ➤ Features
This home sever will provide the following features:
- Discord Bot hosting
- DNS server
- Docker Swarm Management
- IoT
- Smart Home Automation (TBD)
- Minecraft Server
- Webhosting
- Virtual Machines
- VPN

# ➤ Getting Started
## System Requirements
Before you begin, make sure you do the following first:
- Collect the [necessary hardware](https://github.com/sebastienlindqvist/ClusterHat_Server/blob/main/SubSections/Assembly.md)
- Download the necessary [OS](https://clusterctrl.com/setup-software)
- Put [Images](https://www.raspberrypi.com/software/) on SD cards


# ➤ Components
the components used within this repository are:  
1x Raspberry Pi 4B 8gb  
4x Raspberry Pi 0  
1x ClusterHat  
1x Raspberry Pi UPS V5 UPS Plus  
1x 3D printed Case  
2x Acrylic Sheets  
4x Fans  
5x microSD  
2x 18650 batteries  
1x OLED screen

# Assembly
When it comes to assembling the whole thing it is pretty simple. First you need assemble all the electronics. which can be seen in the images below.
### Fully Built Electronics
------
**Underneath**  
<img src="images/PXL_20210809_172128676.jpg" width="533" height="400">  
**Front**  
<img src="images/PXL_20210809_172120149.jpg" width="533" height="400">  
**Back**  
<img src="images/PXL_20210809_172115909.jpg" width="533" height="400">

### OS
--------
Now you need to place the raspbian os the can found on https://clusterhat.com/ on each of the microSD with an Imager. I use the raspberry pi image from the raspberry pi website https://www.raspberrypi.org/software/.  

### Case
----------
When it came to the case. It was designed using SolidWorks and the image below shows the model. This has now been updated a  
<img src="images/case isotropic.png" width="533" height="400">  
The files can be found under the CAD folder for 3D printing. After this we need to connect the 4 fans with soldering to allow it to be attached to the clusterHat board. In the image below it shows where on the board the pins where the fans will be connected to. It shows underneath the board which pin is positive or negative.
The screws supplied with the fans need to be filed down due to being too long. If left too long when moving the cluster in and out the screws collide with the sd cards.


<img src="images/case with cluster in.jpg" width="533" height="400">  

# Setup #
### VNC
Once everything is fully assembled and the right OS is on each of the Raspberry Pis. Plug in a monitor, keyboard and mouse into the Raspberry Pi 4b so you can turn on vnc within the preferences.  
After this the first thing to do is to download vnc viewer on your main computer so you can interact with the Raspberry Pi 4b without needing a separate monitor, mouse and keyboard. Once that is done we can start the next stuff.  
### SSH 
If instead you want to use SSH to connect to your Raspberry Pi 4b that also works. This can be done while burning the image onto the microSD card using Pi Imager. I recommend using PuTTY if you've never done it before.



# CusterHAT
In order to setup the ClusterHAT correctly, you'll be to run the command below on the terminal.
```
clusterhat on
```
this will power all the zeros on the ClusterHAT. This will be indicated by the orange PWR LEDS coming on as well as the blue LED indicating the ClusterHAT board is performing an action.
if you want to apply power to specific Pi zeros, you'll need add the Pi Zero at the end. The example below is for P1
```
clusterhat on p1
```
To switch it off:
```
clusterhat off
clusterhat off p1
```
## Fans
The ClusterHAT board has a couple pins dedicated for a fan, but this is not on by default so you'll need to change your ```/boot/config.txt``` file and add the following to the bottom
```shell
dtoverlay=gpio-fan,gpiopin=18,temp=75000 #this is 75 degrees C
```
Note that the 20000 I put means 20 degree celsius. So when the CPU temperature is above 20 degrees, the fans come on.

I have updated the Server casing. due to soldering issues with soldering 5 wires together. I had to use a single larger fan instead.
## UPS Plus
The UPS I use is called a EP-0136. It uses 2 18650 lithium battery

I highly recommend Michael Klements video: https://youtu.be/clbt12upuaA  
In addition, here is the website for the specific UPS: https://wiki.52pi.com/index.php/EP-0136
# Docker Swarm
What is docker? docker is a online service that uses containers to allow quick and simple deployment of software on a metaphorical port, in this case, the server. What is a container? A container is a standardized box that has everything necessary to run the programs within the container. Imagine how containers can easily stack on top and next to one another on a freight ships. Each container looks exactly the same of the outside but the inside could be anything.

Now that I have given a brief overview of Docker and containers. Now we can talk about Docker Swarm. This is when we have multiple computers running the same containers or we can specify a container to run specifically on one node

to setup the Docker swarm, all you need to do is download Docker and write in the terminal on the computer you want to be the manager.
```
docker swarm init
```
Once that is done, you'll be shown a command to run in the terminals of the computers you want to add to the swarm.

each node can join the swarm by writing this command in the command shell
```
docker swarm join --token [very long token goes here]
```

# Show your support
Give me a ⭐️ if this project helped you!  
Made with 💜 by [me](https://github.com/sebastienlindqvist) 👋


