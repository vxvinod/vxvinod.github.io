# XMSC Dockerization

### _What is Docker?_

Docker allows us to package up the applications/services with all of their dependencies into a standard unit which is called an Image. Here for XMSC automation, Dicker Image will contain all the applications with its dependencies will be loaded.

Without docker – all applications compatible with the OS must be taken care of to build the environment, to resolve this Docker each component runs in a separate container. So a container

Containers – isolated environments process, networks, and mounts share their own OS kernel. It uses LXC containers docker offers a tool to create containers.

### Containers vs Virtual Machines –
VMS – **Hardware , then Hypervisor, VM(OS, libs, app)** -VM's are virtual os where all the application will share the resource in common which in turn consumes high CPU utilization, data storage and Boot up takes more time.

Docker – **Hardware, then OS, Docker manages the containers** - All applications are containerized and it uses the common OS kernal but each container has its own OS, process, resource to manage itself. . So eventually docker has low CPU utilization, usage of data storage and speed boot up.


### Docker commands:

>docker run nginx –  pulls the image from the repo and runs the container.

>docker ps – displays the list of the container with basic information.

>docker stop xxx – stops the container

>docker ps -a – Shows container information with exited containers.

>docker rm xx – remove the containers

>docker images – list all images in the system.

>docker rmi xxx – remove the images.

>docker pull nginx – pulls the image.

>docker run ubuntu – will exit because ubuntu is an image and no service is running. A container lifecycle ends when the service stops or crashes.

>docker run ubuntu sleep 5 – it boots up the container and sleeps for 5 seconds.

>docker exec xxx cat /etc/hosts – runs the command inside the container and prints it.

>docker run -d xxx - detach the containers with the main containers.

### Docker run commands:

>docker run redis:4.0 (:4.0 is tag)

>docker run -it centos bash – gets to container

docker – noninteractive doesn’t wait for the user to enter input.

-I – interactive

-t – attach to sudo terminal

-it – interactive to the terminal. Users can provide input in the terminal

### Docker Port mapping
> **docker run -p 80:5000 redis** - docker maps host 80 port to the docker 5000 port.
 > **docker run -p 8001:3306 mysql** - docker maps 8001 host port to the docker 3306 port.

### Volume mapping - data persistent
Assume a container with mysql is running and User wants to remove the container, BOOOM what happened...? All the data in the mysql DB is gone. To avoid this docker has provided an option to map the host directory to the container directory so that data can be persisted in the host machine and do not worry about the data inside the container even it is destroyed.

### Inspect container
Though **docker ps** command gives the basic information about the container. Inspect commands gives the detailed information about the container in JSON format.

>> docker inspect container_name

### Container logs
>docker logs container_name
logs command will display the stdout logs of the container.
