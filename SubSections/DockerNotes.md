# Docker
What is docker? docker is a online service that uses containers to allow quick and simple deployment of software on a metaphorical port, in this case, the server. What is a container? A container is a standardized box that has everything necessary to run the programs within the container. Imagine how containers can easily stack on top and next to one another on a freight ships. Each container looks exactly the same of the outside but the inside could be anything.

## Installation


## Docker Swarm
Now that I have given a brief overview of Docker and containers. Now we can talk about Docker Swarm. This is when we have multiple computers running the same containers or we can specify a container to run specifically on one node. Similar to Kubernetes.

To setup the Docker swarm, all you need to do is write in the terminal on the computer you want to be the manager.
```
docker swarm init
```
Once that is done, you'll be shown a command to run in the terminals of the computers you want to add to the swarm.

each node can join the swarm by writing this command in the command shell
```
docker swarm join --token [very long token goes here]
```
