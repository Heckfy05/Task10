### Task 10_Docker
For home task 10, we have two separate subtasks.

##### Task 10.1
1. Install docker.
   I used AWS EC2 instance with Ubuntu 22.04 (ami-03e08697c325f02ab) on it. For installing Docker I used instructions from [official sit](https://docs.docker.com/engine/install/ubuntu/)
2. Prepare a dockerfile based on Apache or Nginx image
[Docker-File](https://github.com/Heckfy05/Task10/blob/main/Task1/Dockerfile)        
    >FROM httpd:2.4
    COPY . /usr/local/apache2/htdocs/

    FROM - we specify the base docker image (Apache server) from DockerHub on top of it we will add layers of modification.
COPY - layer that takes [files](https://github.com/Heckfy05/Task10/blob/main/Task1/index.html) from (.) current directory on the host machine and copy into container to the (/usr/local/apache2/htdocs/) 
3. Creating DockerImage from docker file and launching a container based on this image.
   >docker build -t ht10:v11 .
   
   Docker will build docker-image from the docker-file.
   . - current directory set where get docker-file.
   -t - applying tag to image (V11)

   >docker run -d -p 8080:80 bec0cc2c7dfd
   
   Docker will run the container based on the created image.
   -d - deatached mode (backgrounde)
   -p - port frowarding host-8080 container-80
   bec0cc2c7dfd - docker image id

   ![img](https://github.com/Heckfy05/Task10/blob/main/Task1/img/2.jpeg?raw=true)
4. As resoult of launched conteiner we visiting host ip on port 8080
   ![img](https://github.com/Heckfy05/Task10/blob/main/Task1/img/1.jpg?raw=true)
   
##### Task 10.2
1. Prepare private and public networks
   >docker network create internet
   docker network create local --internal
   
   Creating of two separated networks (internet, local)
   --internal Restrict external access to the network
   ![net](https://github.com/Heckfy05/Task10/blob/main/Task2/img/1_network_create.jpeg?raw=true)
2. Prepare one [Dockerfile](https://github.com/Heckfy05/Task10/blob/main/Task2/Dockerfile) based on ubuntu with the ping command
   >FROM ubuntu
    RUN apt-get update && apt-get install -y iputils-ping
    CMD bash

   FROM - we specify the base docker image (Ubuntu) from DockerHub on top of it we will add layers of modification.
   RUN - instruction to update repo and install ping module
   CMD - specifies the intended command for the image
3. Image creation based on [Dockerfile](https://github.com/Heckfy05/Task10/blob/main/Task2/Dockerfile)
   >docker build -t node:v1 .

   ![imag](https://github.com/Heckfy05/Task10/blob/main/Task2/img/2_Image%20create.jpeg?raw=true)
4. Launching two containers (node1 and node2) based on Dockerimage with specified separate networks (node1>internet, node2>local)
   ![image](https://github.com/Heckfy05/Task10/blob/main/Task2/img/3_Launch_Conteiners.jpeg?raw=true)

5.  Connecting node1 container to the local network
    >docker network connect local node1

6. Ping from node1 container to globallogic.com and node2- successful
   ![im](https://github.com/Heckfy05/Task10/blob/main/Task2/img/4_Ping_Node1.jpeg?raw=true)
7. Ping from node2 conteiner to globallogic.com -not successful, to node1 -successful
   ![im](https://github.com/Heckfy05/Task10/blob/main/Task2/img/5_Ping_Node2.jpeg?raw=true)
