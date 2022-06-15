# ClusterHat-Server
Hello, I made this repository for setting up a raspberry server using a cluster hat, a ups. I am by no means confident in what I write, I'm currently in the process of learning it as well. However, if any of it is able to someone then that makes me happy. 
Hopefully I will be useful to maintain this and keep it up to date.

# Components
the components used within this repository are:  
1x Raspberry Pi 8gb  
4x Raspberry Pi 0  
1x CluasterHat  
1x Raspberry Pi UPS V5 UPS Plus  
1x 3D printed Case  
2x Arcylic Sheets  
4x Fans  
5x microSD  
2x 18650 batteries  
1x OLED screen

# Assembly
When it comes to assembling the whole thing it is pretty simple. First you need assemble all the electronics. which can be seen in the images below.
### Fully Built Electronics
Underneath  
<img src="images/PXL_20210809_172128676.jpg" width="533" height="400">  
Front  
<img src="images/PXL_20210809_172120149.jpg" width="533" height="400">  
Back  
<img src="images/PXL_20210809_172115909.jpg" width="533" height="400">  
### OS
Now you need to place the raspbian os the can found on https://clusterhat.com/ on each of the microSD with an imager. I use the raspberry pi image from the raspberry pi website https://www.raspberrypi.org/software/.  
### Case
When it came to the case. It was designed using solidworks and the image below shows the model.   
<img src="images/case isotropic.png" width="533" height="400">  
The files can be found under the CAD folder for 3D printing. After this we need to connect the 4 fans with soldering to allow it to be attached to the clusterHat board. In the image below it shows where on the board the pins where the fans will be connected to. It shows underneath the board which pin is positive or negative.
The screws supplied with the fans need to be filed down due to being too long. If left too long when moving the cluster in and out the screws collide with the sd cards.


<img src="images/case with cluster in.jpg" width="533" height="400">  

# Setup #
### VNC
Once everything is fully assembled and the right OS is on each of the Raspberry Pis. Plug in a monitor, keyboard and mouse into the Raspberry Pi 4b so you can turn on vnc within the preferences.  
After this the first thing to do is to download vnc veiwer on your main computer so you can interact with the Raspberry Pi 4b without needing a seperate moitor, mouse and keyboard. Once that is done we can start the next stuff.  
### SSH 
If instead you want to use SSH to connect to your Raspberry Pi 4b that also works. I recommend using PuTTY if you'v never done it before.

once you have done that now you need to 
```
clusterhat on
```
this will power all the zeros on the 


### CusterHAT
# Docker Swarm
what is docker? docker is a online service that uses containers to allow quick and simple 
