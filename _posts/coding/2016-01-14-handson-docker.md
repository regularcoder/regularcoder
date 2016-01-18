---
layout: post
title:  "Hands on with Docker"
date:   2016-01-14 15:00:00
categories: Docker
---

Last weekend I visisted Google's offices in Cambridge, MA for a Meetup on Docker and Kubernetes.

Here are my notes from the hands on the Docker session that was led by Jonas Rosland, a developer advocate from EMC:

## Containers
We walked through this [presentation](http://www.slideshare.net/jonasrosland/docker-and-containers-overview-docker-workshop) that explains some of the motivations for Docker.

The analogy to *Intermodal Shipping Containers* was enlightening - these are the containers one sees on cargo ships and on trucks all over the world. They're a standard size and as long as you can fit something inside it you can ship it anywhere in the world. 

Docker attempts to do the same thing. It's website says:
> Docker allows you to package an application with all of its dependencies into a standardized unit for software development....This guarantees that it will always run the same, regardless of the environment it is running in.

## How is a container different from a VM?

Virtual Machines run on top of a hypervisor - something that gives each VM the impression that they are running on actual hardware. VMs contain an entire OS that must be booted starting from BIOS. 

Containers on the other hand have just the application and its dependencies. They run as isolated processes on top of the Docker Engine. A single machine can easily host a lot of containers. During the demo Jonas managed to run 20-30 Riak containers easily on his laptop.

## Hands on
1. Since most of the users were on Windows (the only Linux user laughed to himself when they had a show of hands) we used [Docker Toolbox](https://www.docker.com/docker-toolbox) so that everyone could get started quickly.

2. Open a command prompt console and type the following to see a list of virtual-machines on your machine.
   
        docker-machine ls
3. Since we were starting from scratch we deleted anything we saw by running:
        
        docker-machine rm *machine-name*
Where *machine-name* is the name of the VM you see.

4. You can now create your own VM by using the VirtualBox driver. Oracle VM VirtualBox is installed as a part of the Docker Toolbox. Here *handson* is the name of the new virtual machine. 

        docker-machine create -d virtualbox handson
The output that is generated from this command looks something like this:

        Running pre-create checks...
        Creating machine...
        ...
        (handson) Creating VirtualBox VM...
        (handson) Creating SSH key...
        (handson) Starting VM...
        ....
        Provisioning with boot2docker...
        ....
[boot2docker](http://boot2docker.io/) is a light-weight Linux distribution made specifically to run Docker containers.
So far we have created a VM machine that runs the Boot2Docker operating system. If you launch the Oracle VM VirtualBox Manager you will see the machine running:
![Subfolders]({{ site.url }}/img/handson_docker/virtualbox_manager.png)

5. You now need to set some environment variables so that the rest of the commands work properly and go against the right VM that was just created.
        
        docker-machine env handson --shell bash
This will generate output that looks something like this:
        
        export DOCKER_TLS_VERIFY="1"
        export DOCKER_HOST="tcp://192.168.99.100:2376"
        export DOCKER_CERT_PATH="C:\Users\redacted\.docker\machine\machines\handson"
        export DOCKER_MACHINE_NAME="handson"
        # Run this command to configure your shell:
        # eval $("c:\Program Files\Docker Toolbox\docker-machine.exe" env handson --shel
        l bash)
Copy the export commands and paste them into your console window so that they execute.

6. Now when you run the command below you should see both a client and a host. If you don't then it means something went wrong with the earlier steps.

        docker version
Sample output:
        
        Client:
        Version:      1.9.1
        API version:  1.21
        Go version:   go1.4.3
        Git commit:   a34a1d5
        Built:        Fri Nov 20 17:56:04 UTC 2015
        OS/Arch:      windows/amd64
        
        Server:
        Version:      1.9.1
        API version:  1.21
        Go version:   go1.4.3
        Git commit:   a34a1d5
        Built:        Fri Nov 20 17:56:04 UTC 2015
        OS/Arch:      linux/amd64
7. View images of redis key value store on [Docker Hub](https://hub.docker.com/):
        
        docker search redis
![Subfolders]({{ site.url }}/img/handson_docker/redis_dockerhub.png)

8. Download Redis image by calling:

        docker pull redis