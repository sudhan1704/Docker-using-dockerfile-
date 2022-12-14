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
systemctl enable docker

step 5:
create Dockerfile directory under /var/lib/docker <br/>
cd /var/lib/docker/Dockerfile <br/>
mkdir httpd <br/>
cd httpd <br/>
touch Mytestfile <br/>
vi test_httpd_copy <br/>
FROM httpd:latest <br/>
COPY Mytestfile . <br/>
save this file using :wq and exit

step 6:
build new image <br/>
docker build -t myhttpd:1 . -f test_httpd_copy

step 7:
start container <br/>
docker run -d -it -p 8086:80 myhttpd:1

step 8:
validate copied file <br/>
docker exec -it myhttpd /bin/bash <br/>
ls Mytestfile <br/>

It will list the copied file. <br/>


Like this you can test ubuntu OS along with COPY module in docker file <br/>

Test COPY Complete.

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















