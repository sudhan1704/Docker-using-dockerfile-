# STEPS TO TESTING VARIOUS COMMANDS IN THE DOCKERFILE TO BUILD DOCKER IMAGES AND RUN CONTAINERS

## TESTING COPY COMMAND

Step: 1
Disable selinux <br/> 
vi /etc/selinux/config <br/>
SELINUX=disabled

Step: 2
Restart Linux Server

step 3:
Check docker service is running <br/>
systemctl status docker <br/>
Start the service if not running <br/>
systemctl start docker <br/>

#### This command will enable the docker service automatically whenever the system reboots and starts. 
systemctl enable docker

#### What is Dockerfile?

Is a set of instructions that creates a stacked-layer for each instructions that collectively makes a images.Docker image build using layer architure.

#### Commands Inside Dockerfile:

    FROM - defines which image to download and start from
    RUN - We can execute any kind of LINUX commands
    COPY - Used to copy files from host env to inside of container
    ADD - To add multiple file at same time to the container
    CMD - command to be executed when running a container (variable value)
    ENTRYPOINT - container's executable (constant value)
    EXPOSE - Maps/Documents a port into a container

step 5:

#### Docker root directory - /var/lib/docker. The docker root directory can also be mounted seperately.

create Dockerfile directory under docker root directory
cd /var/lib/docker/Dockerfile <br/>
mkdir httpd <br/>
cd httpd <br/>
touch Mytestfile <br/>
vi test_httpd_copy <br/>
FROM httpd:latest <br/>
COPY Mytestfile . <br/>
save this file using :wq and exit

#### Naming convention used for building a docker image - Dockerfile (where docker command are written) 

step 6:
To build a new image <br/>

#### To build docker from current location:

docker build . -t myhttpd:1

#### To build docker from different file name apart from Dockerfile

docker build . -f Mytestfile -t myhttpd:2

step 7:
start container <br/>

docker run -d -it -p 8086:80 myhttpd:1

    -d - detached mode runs container in background
    -it - interactive way of accessing a container through terminal
    -p - port number to access the container. The first port indicates the local port and number after colon indicates container/application port

step 8:
validate copied file <br/>
docker exec -it myhttpd /bin/bash <br/>
ls Mytestfile <br/>

It will list the copied file. <br/>

Like this you can test ubuntu OS along with COPY module in docker file <br/>

Test COPY Complete.

----------------------------------------------------------------------------------------------------------------------------------------------

    Docker File ---------> Docker Image -----------> Docker Container
              
                   BUILD                     RUN
              
----------------------------------------------------------------------------------------------------------------------------------------------

## TESTING ADD COMMAND

step 1:
create Dockerfile directory under /var/lib/docker <br/>
cd /var/lib/docker/Dockerfile <br/>
mkdir testadd <br/>
cd testadd <br/>
vi test_add <br/>
FROM httpd:latest <br/>
ADD test.tar . <br/>
save this file using :wq and exit <br/>
tar -cvf test.tar . <br/>
ls -lrt 

step 2:
build new image <br/>
docker build -t testadd:1 . -f test_add

step 3:
start container <br/>
docker run -d -it -p 8085:80 testadd:1

step 4:
validate execution of the add command <br/>
docker exec -it <container_id> /bin/bash <br/>
ls  <br/>
It will list the extracted tar file.

Test ADD complete

---------------------------------------------------------------------------------------------------------------------------------------------------

## TESTING RUN, CMD, ENTRYPOINT 

step 1:
create Dockerfile directory under /var/lib/docker <br/>
cd /var/lib/docker/Dockerfile <br/>
mkdir test_sample <br/>
cd test_sample <br/>
vi test_commands <br/>
FROM alpine:latest <br/>
RUN apk add python3 <br/>
CMD ["192.45.0.11"] <br/>                 
ENTRYPOINT ["ping", "-t", "4"] <br/>
save this file using :wq and exit <br/>

step 2:
build new image <br/>
docker build -t testcommands:1 . -f test_commands <br/>
(The image will be built in 4 steps in the order mentioned in the dockerfile)

step 3:
start container <br/>
docker run -d -it -p 8084:80 testcommands:1

step 4:
To check ping command <br/>
docker run testcommands:1 192.68.0.45 <br/>
#Input the ip of the server to ping 

Test RUN, CMD, ENTRYPOINT complete

-----------------------------------------------------------------------------------------------------------------------------------------------------

## DOCKER EXPOSE

The expose command in a Dockerfile tells Docker that a container listens for traffic on the specified port. 
So, for a container running a web server, adding this to the Dockerfile: EXPOSE 90. 
This tells the Docker that the webserver will listen on port 90 for TCP connections though port 80 is the default TCP port.

step 1:
create Dockerfile directory under /var/lib/docker <br/>
cd /var/lib/docker/Dockerfile <br/>
mkdir testexpose <br/>
cd testexpose <br/>
vi test_expose <br/>
FROM httpd:latest <br/>
EXPOSE 90 <br/>
save this file using :wq and exit <br/> 

step 2:
build new image <br/>
docker build -t testexpose:1 . -f test_expose 

step 3:
start container <br/>
docker run -d -it -P testexpose:1

step 4:
To check if the container is running with the port mentioned in the dockerfile <br/>
docker ps                                   #command for listing running containers<br/>
It will display the port opened in the running containers

Test EXPOSE complete.

-----------------------------------------------------------------------------------------------------------------------------------------------

    docker info # to check information about docker like root directory
    docker images # will list the available images
    docker ps # shows the current running container with ports assigned
    docker ps -a # show the hidden container which are in exited status
    docker inspect < image ID or Container ID > # For research purpose (if the docker image is already created by someone to know what exactly written in it,
    to know the container's IP and workign directory we can use this command
-----------------------------------------------------------------------------------------------------------------------------------------------
 #### Docker Volume:
 
In order to preserve data generated by a running container, docker volume is used to mount on containers. In case the container no longer exist or accidently
removed by someone the data inside that container can be accessed from the mounted file system while starting that particular container
 
    docker container run -d -p 8080:80 -it -v /volume/test:/usr/local/apache2/htdocs httpd:latest
    
This can be verified using command
    
    docker inspect < container ID >
    
-----------------------------------------------------------------------------------------------------------------------------------------------

#### HouseKeeping - Docker Images and Containers

In order to stop the running containers and remove its corresponding images. First to stop the running container use
 
    docker container stop < container ID >
    
Once stopped, to remove the container

    docker container rm < container ID >
    
To stop & remove multiple containers

    docker container stop $ (docker container ls -aq) # will list all the available containers
    
    docker container rm  $ (docker container ls -aq)
    
 After container is stopped and removed, we can remove the images either one by one or at single shot
 
    docker images rmi < image ID >
    
    docker images rmi $(docker image ls -aq)
    
##### Don't use in office environment:

    docker container prune # will remove all the stopped, running and hidden container







