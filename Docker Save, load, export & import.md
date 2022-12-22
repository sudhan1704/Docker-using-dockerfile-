## Docker save

Save one or more images to a tar archive. A Docker image is a kind of template, built from a Dockerfile, that specifies the layers required to build and run an app.
To share or back up our image, we use the docker save command.

    $ docker save [image_name] > ./[saved_image_name].tar
    
    Example :
    $ docker save centossave:1 > ./centossave.tar
    
    
## Docker load

To load an existing image, we use the load command. 
This command loads an image or repository from a tar archive. 
It restores both images and tags.

    $ docker load < [saved_image_name].tar
    
    Example :
    $ docker load < centossave.tar
    
    
## Docker export

Export a container’s filesystem as a tar archive. The docker export command does not export the contents of volumes associated with the container. 
If a volume is mounted on top of an existing directory in the container, docker export will export the contents of the underlying directory, 
not the contents of the volume.

  - Export as a local file
  
        $ Docker export [container_id] > ./[Filename].tar
        
        Example:
        Docker export 934nsdafo3r9 > ./export.tar
        
    
    
    
## Docker import


Import the contents from a tarball to create a filesystem image

  - Import from a local file

        $ cat [filename].tar | docker import - [filename:version]
    
        Example:
        $cat export.tar| docker import - layers:3
    
  - Import from a remote location
  
        $ docker import https://example.com/exampleimage.tar









