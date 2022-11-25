# Compose Files
here are the docker compose files I have used within my home server and all updated to docker compose version 3.9


# Homer
homer is a full static html/js dashboard generator. I used this to take the webpage for the whole server so I can access all the applications I have running as well as access to other applications on my local internet such as my 3D printer.

When it came to using Docker directly did not work for me. Creating a docker-compose.yml file did not work for me either because it told me "it does not support that version". What did work for me was creating a stack on Portainer and writing in the same docker-compose.yml file. In addition, when it did work I noticed it did not have a port set for it because I deployed it to the swarm in general. So I altered the docker-compose file to specify I want it purely on the Pi 4 and that allowed me to access it. 

In order to change how it looks, you'll need to set a path to find the necessary files on the host. In the image below you will see where you need to add a path.

Once you've done that, you can place the config.yml file there and all the necessary additions such as images and css files.

![Homer](../images/Homer.png)



# PiHole
One of the things I have running on the server is PiHole, Currently on the main Pi 4B. I tried running it on the zeros but due to how the IP address was done internally. It would not work. In the future I will try and have this on Docker or Kubernetes so that it can run on the zeros without issue.

![PiHole](../images/PiHole.png)

All you have to do is go to the PiHole website/Github and run
```
curl -sSL https://install.pi-hole.net | bash
```
This can be done as well as by deploying the container that hosts PiHole as well
Which is what I will be doing in the future

If you ever forget your password to PiHole. Got to....
and write the following code
``` 

```

# ShellInABox
This software allows you to access shell via your browser rather using VNC or ssh.
```
sudo apt install ShellInABox
```
once that is done, simply type what is below into your browser.  
```https://[yourip]:4200```

