Step: 1
Disable selinux <br/>
vi /etc/selinux/config
SELINUX=disabled

Step: 2
Restart Linux Server

step 3:
Check docker service is running <br/>
systemctl status docker
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
docker build -t myhttpd:1 .

step 7:
start container <br/>
docker run -d -it -p 8086:80 myhttpd:1

step 8:
validate copied file <br/>
docker exec -it myhttpd /bin/bash <br/>
ls Mytestfile <br/>

It will list the copied file. <br/>


Like this you can test ubuntu OS along with COPY module in docker file <br/>

Test Complete
