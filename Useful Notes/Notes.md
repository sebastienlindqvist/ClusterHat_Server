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

```
clusterhat on p1
minicom p1
sudo raspi-config
```
### Code from other websites

# Docker Notes
to install docker on raspberry pi 4b: https://dev.to/elalemanyo/how-to-install-docker-and-docker-compose-on-raspberry-pi-1mo


to install docker on raspberry pi 0: https://markmcgookin.com/2019/08/04/how-to-install-docker-on-a-raspberry-pi-zero-w-running-raspbian-buster/  
the pi 0 runs an ARM6




# UPS plus SKU
https://wiki.52pi.com/index.php/UPS_Plus_SKU:_EP-0136?spm=a2g0o.detail.1000023.17.4bfb6b35vkFvoW
```

```
