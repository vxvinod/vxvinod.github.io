---
layout: post
title:  "Docker Images - Great way to build the Image"
date:   2023-02-28 
desc: "The heart of docker is the image we build using dockerfile and we are here to explore various commands and techniques to build docker images with its options"
---
## Docker Image
 Docker images are built with custom dependencies of software and applications(chrome, curl, spotify), it is a combination of OS and its dependencies which is not available at docker hub.

#### Steps to build docker image - Write down steps to deploy manually flash application.

1.  OS – Ubunutu
2.  Update the source repository using apt command – update apt repo
3.  Install dependencies using apt.
4.  Install python dependencies using pip command.
5.  Copy source code to /opt folder.
6.  Run the web server using ‘flash’ command.

#### Docker file
Docker file is a file which we can specify the instruction and its argument to build a image. Some of the instructions are RUN, COPY, ENTRYPOINT, CMD.
#### Docker file for flash application

>> From Ubuntu
>>Run apt-get update
>>RUN apt-get install python
>>RUN pip install flask
>>RUN pip install flask-mysql
>>COPY . /opt/source-code
>>ENTRYPOINT FLASK_APP=/opt/source-code/app.py flask run

#### Docker Image build command
>>**docker build Dockerfile -t image_name**
Above command build docker image as mentioned in Dockerfile and name it as docker image name mentioned.
#### Docker push Command
>> **docker push image_name**
>command will push the docker image in to the docker hub.

#### Docker Layered Architecture
Docker builds images in laa yered architecture. So when a command fails the previous successful image built is cached and when we try to rebuild the image, It starts from the last successful layer.
#### Docker History
It allows to view the size of the image and its information.

>>  **docker history image_name**
command helps to view the history of the image name with its size and details.

>> Command to check the linux flaour - **cat /etc/*-release**

#### Docker Setting Environment Variables:

Docker environment variables can be passed through -e argument when running a container.

>> **docker run -e APP_COLOR=blue image_name**

>> **docker inspect container_name** – will list the container name with all config and environment variable which is present inside the configuration.

##### Docker command to start mysql and name and pass the env variables.
>> docker run -d --name mysql-db -e MYSQL_ROOT_PASSWORD=db_pass123 mysql
