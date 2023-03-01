# ClusterHAT notes 

## CusterHAT
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
### Code from other github ###
https://github.com/lreimer/raspi-swarm-box
Before you continue with the following steps, make sure you have performed the basic setup of all four Zero nodes.
In a terminal, issue the following commands:
```
clusterhat on

ssh-keygen
ssh-copy-id -i ~/.ssh/id_rsa pi@172.19.181.1
ssh-copy-id -i ~/.ssh/id_rsa pi@172.19.181.2
ssh-copy-id -i ~/.ssh/id_rsa pi@172.19.181.3
ssh-copy-id -i ~/.ssh/id_rsa pi@172.19.181.4
```
To make working with the nodes a little easier, edit your `/etc/hosts` file and add the following:
```
172.19.181.254  master cnat
172.19.181.1    p1
172.19.181.2    p2
172.19.181.3    p3
172.19.181.4    p4
```
#### Zero Node Setup

On the controller node, open a terminal and perform the following commands for each Zero (p1, p2, p3, p4).
The user is `pi` and the initial password is `clusterhat`.
```
clusterhat on p1
minicom p1
sudo raspi-config
```
In the configuration,
- make sure you extend the partition to the full size of your SD card!
- enable SSH
- change the root password
- reduce the memory split to 16MB
- adjust the locale and timezone settings
- update the system

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
    Hostname 172.19.181.1
    User pi
Host p2
    Hostname 172.19.181.2
    User pi
Host p3
    Hostname 172.19.181.3
    User pi
Host p4
    Hostname 172.19.181.4
    User pi
```
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
