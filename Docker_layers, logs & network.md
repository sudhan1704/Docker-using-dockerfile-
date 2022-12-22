# Docker layers

Layers are a result of the way Docker images are built. 
Within the image, filesystem content is represented as multiple independent layers.
Each step in a Dockerfile creates a new “layer” that’s essentially a diff of the filesystem changes since the last step. 
Metadata instructions such as LABEL and MAINTAINER do not create layers because they don’t affect the filesystem.

 - Example
 
    This image has 4 instructions (RUN) so it’ll create 4 layers excluding the centos layer:
    
      From centos:7.9.2009
      RUN yum update all -y
      RUN yum install java -y
      RUN yum install python -y
      RUN yum install net-tools -y
      
      The first step runs the yum command to update all the packages thats based on centos 7.9.2009 image into a new layer.
      
      The second step runs the yum command to install java. This creates a second layer based on the first one.
      
      Third step runs the yum command to install python and fourth step runs the yum command to install net-tools creating the third and fourth layer respectively.
      
      Each build step emits the ID of the created layer. The last step’s layer becomes the final image.
      
      FROM instructions are a special case that reference the final layer of an existing image.
 ---------------------------------------------------------------------------------------------------------------------------------------------     
      
## Inspecting Image Layers

You can list the layers within an image by running the docker image history command. 
Each layer displays the ID of the created image and the Dockerfile instruction that caused the change. 
You can see the total size of the content within the layer too.

    $ docker image history
-------------------------------------------------------------------------------------------------------------------------------------

## Reducing the size of the image to be build

Clubbing all the RUN instructions in a single layer

      From centos:7.9.2009
      RUN yum update all -y && \
      yum install java -y && \
      yum install python -y && \
      yum install net-tools -y
      
The example above creates just two layers on top of the centos 7.9.2009 image instead of five.

Layers use space and as the levels increase, the final image size also increases. 
This is because the system keeps all the changes between the various statements.
It may be a good practice to combine the various instructions to form a single layer to reduce the size of the image.

-----------------------------------------------------------------------------------------------------------------------------------

## Docker logs

Fetches the logs of a container.

  -  The basic syntax to fetch logs of a container is:
            
    $ docker logs <CONTAINER-NAME OR ID>
    
Docker, by default, captures the standard output (and standard error) of all your containers and writes them in files using the JSON format. 
This is achieved using JSON File logging driver or json-file. 
These logs are by default stored at container-specific locations under /var/lib/docker filesystem.

    /var/lib/docker/containers/<container_id>/<container_id>-json.log
    
---------------------------------------------------------------------------------------------------------------------------------------------

## Network containers

To create a network bridge

     $ docker network create -d bridge <network_name>
     
     Example : $ docker network create -d bridge mybridge
     
    
To list the networks

    $ docker network ls
    
    
  - Example result:

        NETWORK ID          NAME                DRIVER
        7b369448dccb        bridge              bridge
        615d565d498c        my_bridge           bridge
        18a2866682b8        none                null
        c288470c46f6        host                host

To build and run a container in a created network 

    $ docker run -it -d -p <port> --network=<network_name> <image_name>
    
    Example : $ docker run -it -d -p 8092:80 --network=mybridgenetwork busybox

To check the ip and the network of the container

    $ docker inspect <container_id>




   



























     
      
      
      
