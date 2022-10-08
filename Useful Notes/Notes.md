# ClusterHAT notes 
### Code from ClusterHAT website ###
```
$ clusterctrl on # Turn power to all Pi Zero on
$ clusterctrl off  Turn power to all Pi Zero off
$ clusterctrl on p1 # Turn power on to Pi Zero in slot P1
$ clusterctrl on p1 p3 p4 # Turn power to Pi Zeros in slot P1, P3 and P4 on
$ clusterctrl off p2 p3 # Turn power off to Pi Zeros in slots P2 and P3
$ clusterctrl alert on # Turns on ALERT LED
$ clusterctrl alert off # Turns off ALERT LED

$ clusterctrl hub on # Turns on USB hub (default)
$ clusterctrl hub off # Turns off USB hub
$ clusterctrl led on # Enables Power & P1-P4 LED on Cluster HAT (default) 
$ clusterctrl led off # Disables Power & P1-P4 LED on Cluster HAT (does not disable ALERT LED)
$ clusterctrl wp on # Write protects HAT EEPROM
$ clusterctrl wp off # Disables EEPROM write protect (only needed for updates)
```


When all changes are done, perform a reboot of the node. You should be able to login via SSH now.
### Code from other websites ###
https://medium.com/@dhuck/the-missing-clusterhat-tutorial-45ad2241d738
After the Pi boots back up, set up a SSH key with the following commands:
```
ssh-keygen -t rsa -b 4096
cat ~/.ssh/id_rsa.pub
```
After you have generated a ssh-key and displayed it, copy it because you will need this in the following steps. The ```.pub``` means your public key.
Set up a config file for SSH names ```~/.ssh/config``` with the following content:
```
Host p1
    Hostname IP_Address
    User pi
Host p2
    Hostname IP_Address
    User pi
Host p3
    Hostname IP_Address
    User pi
Host p4
    Hostname IP_Address
    User pi
```
the IP_Address can be found with Advanced IP Scanner

This previous step is a little superfluous, but you will now be able to quickly ssh into any of the nodes from the controller Pi with the command ```ssh hostname``` where ```hostname``` is the hostname of the nodes: p1, p2, p3, or pi4
You should now be able to ssh into each of the nodes to set them up. Repeat the password and localization set up for each node that you performed for the controller node. You should also put the ssh-key that you copied earlier into the ```~/.ssh/authorized_keys``` file:
```
ssh p1
# the default password for the nodes is clusterctl
echo [paste the ssh key here] >> ~/.ssh/authorized_keys
sudo raspi-config
```
After setting up the localization and ssh on each of the nodes, install the ```ntpdate``` package. This will keep the systemtime synced across the nodes in the background:
```
sudo apt-get install -y ntpdate
```
Hostnames
This is a good time to decide on a hostname schema. By default, the images from ClusterHat uses the scheme ```p[1–4]``` for the nodes and ```controller``` for the controller Pi. I have kept this schema for my cluster. However, since we will be adding some resources from the controller Pi to the cluster, we should go ahead and change the hostname of this Pi to ```p0``` in order to make life much easier.
```
sudo hostname pi0 
sudo vi /etc/hostname # change the hostname in this file
sudo vi /etc/hosts # change ‘controller’ to pi0
```
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
### More websites ###
https://code.metcarob.com/node/174

### More websites ###
https://xaviergeerinck.com/post/infrastructure/clusterhat-setup

# Docker Notes
to install docker on raspberry pi 4b: https://dev.to/elalemanyo/how-to-install-docker-and-docker-compose-on-raspberry-pi-1mo


to install docker on raspberry pi 0: https://markmcgookin.com/2019/08/04/how-to-install-docker-on-a-raspberry-pi-zero-w-running-raspbian-buster/  
the pi 0 runs an ARM6




# UPS plus SKU
https://wiki.52pi.com/index.php/UPS_Plus_SKU:_EP-0136?spm=a2g0o.detail.1000023.17.4bfb6b35vkFvoW
```

```
