# Assembly

## Components
the components used within this repository are:  
- 1x Raspberry Pi 4B 8gb  
- 4x Raspberry Pi 0  
- 1x CluasterHat  
- 1x Raspberry Pi UPS V5 UPS Plus  
- 1x 3D printed Case  
- 2x Arcylic Sheets  
- 4x Fans  
- 5x microSD  
- 2x 18650 batteries  
- 1x OLED screen

## Instructions
1. Flash your SD cards with the necessary images
2. Place the SD cards in the appropriate Raspberry Pi
3. Attached the heat sinks to the Raspberry Pis
4. Attach the ClusterHAT to the Raspberry Pi 4B
5. Attach the Raspberry Pi Zero's in correct order
6. Solder 


### ➤ Assembly
When it comes to assembling the whole thing it is pretty simple. First you need assemble all the electronics. which can be seen in the images below.
### Fully Built Electronics
------
**Underneath**  
<img src="../images/PXL_20210809_172128676.jpg" width="533" height="400">  
**Front**  
<img src="../images/PXL_20210809_172120149.jpg" width="533" height="400">  
**Back**  
<img src="../images/PXL_20210809_172115909.jpg" width="533" height="400">

### OS
--------
Now you need to place the raspbian os the can found on https://clusterhat.com/ on each of the microSD with an Imager. I use the raspberry pi image from the raspberry pi website https://www.raspberrypi.org/software/.  

### Case
----------
When it came to the case. It was designed using SolidWorks and the image below shows the model. This has now been updated a  
<img src="../images/case isotropic.png" width="533" height="400">  
The files can be found under the CAD folder for 3D printing. After this we need to connect the 4 fans with soldering to allow it to be attached to the clusterHat board. In the image below it shows where on the board the pins where the fans will be connected to. It shows underneath the board which pin is positive or negative.
The screws supplied with the fans need to be filed down due to being too long. If left too long when moving the cluster in and out the screws collide with the sd cards.


<img src="../images/case with cluster in.jpg" width="533" height="400">  




## UPS Plus
The UPS I use is called a EP-0136. It uses 2 18650 lithium battery

I highly recommend Michael Klements video: https://youtu.be/clbt12upuaA  
In addition, here is the website for the specific UPS: https://wiki.52pi.com/index.php/EP-0136