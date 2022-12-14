## To run a container in a seperate file system

Default path for the volume mounted in a running container --- /var/lib/docker

To change the default volume mounted on a container (volume mount)

    Syntax
        docker run -d -p <portnumber> -v <new_mount_path>:<local_path_of_the_container> <image_name:version>  

    Example
        docker run -d -p 8090:80 -v /app/webserver:/usr/local/apache2/mydocs httpd:latest
        
A new container will be created with the mounted volume.

#To check if the created container is running use

    docker ps
    
The information of the mounted volume can be confirmed by docker inspect.

## Docker inspect

Docker inspect provides detailed information on the containers controlled by Docker.

    syntax
        docker inspect <container_id>

