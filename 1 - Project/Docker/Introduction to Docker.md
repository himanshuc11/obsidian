Problem: The code works on one machine, but doesn't work on another.

### Virtualization
Virtualization is process of creating an abstraction layer over host hardware, dividing the hardware resources into virtual machine, with each virtual machine running it's own OS and behaving like a independent computer.

Hypervisor is the layer that interfaces underlying hardware and virtual machines
There are two types of hypervisor
1.  Type 1 Hypervisor are bare metal hypervisors that interact with underlying physical resources directly, replacing traditional OS
2. Type 2 Hypervisor are software hypervisors
![[Screenshot 2024-09-17 at 12.46.03 AM.png]]

### Containerization
Containerization is the process of bundling application source code along with all it's dependencies to run on any infrastructure. Since they don't include the OS layer**, they are faster than VMs.

In Docker,
If Unix based OS
- The kernel is shared between them, with layering, we just incrementally add the OS (excluding the kernel). This particular OS uses the host OS kernel.

If Windows based OS
- To simulate Unix infrastructure, a WSL2 which is a Linux Kernel built into windows or hyper-v which allows you to run multiple OS as a VM.
- To simulate windows infrastructure, we still need Windows server (Not sure, need further research, but since we are deploying mostly on linux server, non issue)


![[Screenshot 2024-09-17 at 12.45.51 AM.png]]

There are many ways to containerize an application
1. Linux Containerization
2. Podman
3. Hyper-V (for windows)
4. Docker

Why containerize?
- Run applications in isolated environments (Sandbox)
- Portability
- Ease of deployment
- Developer environment clean (node12, node14, node14 nvm etc)

Each container is made from these 3 parts
1. Manifest File: This is the file to describe the image ie how to build the image from scratch.
2. Image: Image is the standalone application, packaged with all the dependencies to run the application.
3. Container: Docker container is the running instance of an image. (Lightweight virtual machine)
![[Screenshot 2024-09-17 at 11.10.29 AM.png]]

So images are executed so that containers are created.
The same image can be run multiple times, creating a new container when run.

Mental Model:
Manifest file is interface
Image is class
Container is object
### Docker
Docker refers to the Docker Engine, that is used to containerize an application. Same benefit of containerization applies.

Architecture of docker:

**Docker Client**
Docker client is a way to interact with the docker engine. The client can be
1. CLI (Most commonly used)
2. GUI
3. REST API

**Docker Engine**
The actual engine used to build images and containerize applications. It has the actual Docker daemon, which manages all the containers and images.

**Docker Registry**
Docker Registry is a place to host images. There are many different docker registry but the official maintained by docker is called dockerhub

![[Screenshot 2024-09-17 at 12.06.03 PM.png]]

To build containers we need images. These images can be
1. pre built images, pulled from the docker repositories.
2. custom images, built locally.

**Pre built images**
To create containers out of prebuilt images, we just need to run the official image name. 
Just search the image you want to run from https://hub.docker.com/ 

Note: The remote is already set up no need to set remote like github
```bash
docker pull <image-name> // Pull the image from remote to local
docker images // Shows the list of images you have downloaded locally
docker rmi <image-id> --force // Delete the image from your local machine
docker build -t <custom-image-name> /path/to/src
```


Container
```bash
docker ps // Shows the list of containers that are currently running
docker run <image-name> // Creates a container out of the image
docker run -d <image-name> // Runs the container in detached mode (background)
docker run -p port-on-machine:port-exposed-by-docker <image-name> // Allows port mapping, so that we can interact with the docker port
docker run --name <container-name> <image-name> // name the container before running it
docker kill <container-id> // Kill the container that was running.
```
Why detached mode?
Something like server running consistently in the background. I don't want the logs to pollute my terminal.

How to interact with container running in detached mode?
```bash
docker exec <container-id> command // ssh into the container, run the command and close the connection
docker exec -it <container-id> command // ssh into the container and keep the connection running
```


Docker does not persist data across restarts or 
```bash
docker run mongo
docker exec -it <mongo-container-id> /bin/bash
mongosh

db.createCollection("movies")
db.movies.insertOne({name: "Kill Bill"})
db.movies.find()

Kill the container and read
```


This command does the following things
```javascript
1. Searches for the <image-name> locally to see if it's present.
	1. If present, create the container out of the image
	2. If absent, look at the remote to find the <image-name>
		1. If present, pull the image and then create the container (execute docker pull <image-name>)
		2. Else return error


if(image_name is present locally) {
	// run the container
}
else {
	if(checkForImageRemotely(image_name)) {
		pullImageFromRemote(image_name); // docker pull <image-name>
	}
	else {
		throw Error("Image not found");
	}
	
}
```

```bash
docker run node
```

![[Screenshot 2024-09-17 at 2.12.01 PM.png]]
To see which version node was installed
```bash
docker run node -v
```
In my case it showed `v22.8.0`
Creating a `index.js` file and executing it with node.
![[Screenshot 2024-09-17 at 2.27.01 PM.png]]
This gives an error.
The file is created inside host machine, not inside container. To create inside container
- Start the container
- `docker exec -it <container-id> /bin/bash` into it.
- Then `touch index.js` and `echo "console.log('Hello Docker')" >> index.js` and `node index.js` inside it.

### Creating custom images
To make sure when we setup the container with all the custom file, folder etc we can use custom images, instead of doing `docker exec -it <container-id> /bin/bash` and setting it up ourself.

`Dockerfile` is the manifest file that is used to create the image. It is a text file describing the image.
(Dockerfile -> Image -> Container)

It has a 
1. base image
2. commands to run on the base image to convert it into your custom image

```dockerfile
FROM node:latest                 // Base image
COPY index.js code/              // Copy from src to destination
CMD ["node", "code/index.js"].   // Startup command run after setting uf files
```

### Port mapping
Docker containers are run in isolation from the rest of the host and external network.
So the port exposed by docker is actually internal to the container.

So curl request inside machine would work, but request from host will not work
To make sure that request from host works, we need port mapping.

Port mapping binds the port of host machine to the docker machine
localhost:host-port -> localhost:docker-exposed-port.

![[Screenshot 2024-09-18 at 2.26.06 PM.png]]

### Networking
Since docker exposes ports internally, to make sure 2 or more docker containers talk to each other we need networks. 

```bash
docker network create <network-name>
docker run --name <container-name> --network <network-name>
```

### Docker compose
To run multiple docker container we use docker hub.

### Storage solutions
By default docker doesn't persist data when container no longer exists. Container killed => data is lost. Hence we have 2 solutions to persist data (Bind Mounts and Volumes)

![[Pasted image 20240918120136.png]]

There are 3 places to store data
1. Bind Mount: It uses the host machine fs. (Not managed by docker, which means other process can update it and change will be reflected in the container)
2. Volume: It uses a special area controlled and managed by docker inside the host fs. (Managed by docker hence more optimized)
3. tempfs: Uses memory (RAM) of the machine instead of disk. Data that doesn’t need to persist beyond the lifespan of the container.

We will use volumes, because as per docker docs, volumes is the more secure and optimized solution.

Data is retained in a volume, which can be attached to a container.
```bash
docker volume create <volume-name>. // Create a persistent data store
docker run -v <volume-name>:/path/to/data. // Attach volume based on architecture of underlying system.
```

Same volume can be mounted on different containers.

