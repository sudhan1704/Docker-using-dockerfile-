Step: 1
Disable selinux \n
vi /etc/selinux/config
SELINUX=disabled

Step: 2
Restart Linux Server

step 3:
Check docker service is running \n
systemctl status docker
Start the service if not running \n
systemctl start docker
systemctl enable docker

step 5:
create Dockerfile directory under /var/lib/docker
cd /var/lib/docker/Dockerfile
mkdir httpd
cd httpd
touch Mytestfile
vi test_httpd_copy
FROM httpd:latest
COPY Mytestfile .
save this file using :wq and exit

step 6:
build new image
docker build -t myhttpd:1 .

step 7:
start container
docker run -d -it -p 8086:80 myhttpd:1

step 8:
validate copied file
docker exec -it myhttpd /bin/bash
ls Mytestfile

It will list the copied file.


Like this you can test ubuntu OS along with COPY module in docker file
