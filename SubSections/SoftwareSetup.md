# Software Set Up

## VNC
Once everything is fully assembled and the right OS is on each of the Raspberry Pis. Plug in a monitor, keyboard and mouse into the Raspberry Pi 4b so you can turn on vnc within the preferences.  
After this the first thing to do is to download vnc viewer on your main computer so you can interact with the Raspberry Pi 4b without needing a separate monitor, mouse and keyboard. Once that is done we can start the next stuff.  
## SSH 
If instead you want to use SSH to connect to your Raspberry Pi 4b that also works. This can be done while burning the image onto the microSD card using Pi Imager. I recommend using PuTTY if you've never done it before.

## Fan
The ClusterHAT board has a couple pins dedicated for a fan, but this is not on by default so you'll need to change your ```/boot/config.txt``` file and add the following to the bottom
```shell
dtoverlay=gpio-fan,gpiopin=18,temp=75000 #this is 75 degrees C
```
Note that the 20000 I put means 20 degree celsius. So when the CPU temperature is above 20 degrees, the fans come on.

I have updated the Server casing. due to soldering issues with soldering 5 wires together. I had to use a single larger fan instead.

## Memory partitioning
It is not necessary for you to have a shared drive to run commands on the cluster, but it be extremely useful to have a partition to hold data that the entire cluster can use. You can set up and use a partition on the controller’s SD card if it is simpler for you. For my cluster, I used an extra 128 GB thumb drive I had laying about to use as extra drive. Originally, I was planning on using an older 320GB external, but after plugging it in, the controller Pi started giving me under-voltage warnings.  
We will be setting up a NFS share with the controller Pi, so log into the Pi and follow these steps.  
To find out where the device is loaded in ```/dev```, run the ```lsblk``` command and you should see something like this:
```
$ lsblk
NAME MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
sda 8:0 1 119.5G 0 disk
└─sda1 8:1 1 119.5G 0 part
mmcblk0 179:0 0 29.7G 0 disk
├─mmcblk0p1 179:1 0 256M 0 part /boot
└─mmcblk0p2 179:2 0 29.5G 0 part /
```
For me, and most likely yourself, the device is located at ```/dev/sda1```. Next we will need to partition the drive:
```
sudo mkfs.ext4 /dev/sda1
```
After formatting the drive, set up a directory to mount it to. I prefer using the ```/media``` folder to set up my external drives, but you can place the folder anywhere on the filesystem. Just be sure that it will be the same folder across all of your nodes!
```
sudo mkdir /media/Storage
sudo chown nobody.nogroup -R /media/Storage
sudo chmod -R 777 /media/Storage
```
After setting this up, run the ```blkid``` command to get the UUID of the drive so we can set up automatic mounting of the drive whenever the pi boot. You will be looking for a line like this:  
```/dev/sda1: LABEL=”sammy” UUID=”a13c2fad-7d3d-44ca-b704-ebdc0369260e” TYPE=”ext4" PARTLABEL=”primary” PARTUUID=”d310f698-d7ae-4141-bcdb-5b909b4eb147"```  
Though the most important part is the UUID, ```UUID=”a13c2fad-7d3d-44ca-b704-ebdc0369260e”```. Edit your fstab to contain the following line, while making sure you substitute your drives UUID in the appropriate location:  
```
sudo vi /etc/fstab
# Add the following line to the bottom of the fstab file:
UUID=a13c2fad-7d3d-44ca-b704-ebdc0369260e /media/Storage ext4 defaults 0 2
```  
Ensure that the NFS server is installed on your controller:  
```sudo apt-get install -y nfs-kernel-server```  
You will then need to update your ```/etc/exports``` file to contain the following line at the bottom:  
```
/media/Storage 172.19.181.0/24(rw,sync,no_root_squash,no_subtree_check)
```  
After editing the exports file, run the command ```sudo exportfs -a``` to update the NFS server.  
### Mount the Drive on the Nodes ###
You will now need to mount the NFS share that we just set up on each of the node Pis. Run the following command set on each node:  
```
sudo apt-get install -y nfs-common
# Create the mount folder, using the same mount folder above.
# If you used different permissions, use the same permissions here.
sudo mkdir /media/Storage
sudo chown nobody.nogroup /media/Storage
sudo chmod -R 777 /media/Storage
# Set up automatic mounting by editing your /etc/fstab:
sudo vi /etc/fstab
# Add this line to the bottom:
172.19.181.254:/media/Storage /media/Storage nfs defaults 0 0
```