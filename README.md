# Docker

## Running a containerized Java application

`docker run milkyway/java-hello-world`

## Network and ports

### List network types

`docker network ls`

### Create a new network

`docker network create myNetwork`

### inspect a network

`docker network inspect myNetwork`

### Run Apache Tomcat for the newly created network

`docker run -it --name myTomcat --net=myNetwork tomcat`

### Run a busybox with the same network as myTomcat container

`docker run -it --net container:myTomcat busybox`

### Check if we can reach the tomcat webserver from the busybox

`wget localhost:8080`

### Bind your host port to the container

`docker run -it --name myTomcat2 --net=myNetwork -p 8080:8080 tomcat`

### Map a random port to the exposed container port

`docker run -it --name myTomcat3 --net=myNetwork -P tomcat`

### Find out the randomly allocated port

`docker port myTomcat3`
OR check in:
`docker inspect myTomcat3`

## Volumes

### Specify a local path for container volume

Windows: `docker run -v d:/docker_volumes/volume1:/volume -it busybox`

Mac:`docker run -v ~/volume1:/volume -it busybox`

### Create a standalone named volume 

`docker volume create --name myVolume`

### Map the volume to containers

`docker run -it -v myVolume:/volume --name myBusybox3 busybox`

`docker run -it -v myVolume:/volume --name myBusybox4 busybox`

OR use -volumes-from

`docker run -it -volumes-from myBusybox4 --name myBusybox5 busybox`

### Remove a volume

`docker volume rm myVolume`

## Dockerfile

### Build the docker with the Dockerfile provided in this repository
`docker build . -t rest-example`

### Run the newly built image (rest-example)

`docker run -p 8080:8080 -it rest-example `

### Build the ping-example container from Dockerfile

`cd ping-example`

`docker build -t ping-example .`

`docker run ping-example`

for sending ping to localhost

`docker run ping-example www.google.com`

to ping Google

### Run container in detached mode

`docker run -d -p 8080:8080 rest-example`

### Attach to the detached container

`docker attach rest-example`

### View logs of the running container

`docker logs -f rest-example`

### Specify the log driver to write logs to

`docker run log-driver=syslog rest-example`

You can use `journald`, `splunk`, `fluentd`, `awslogs`, etc

### Inspect a container

`docker inspect rest-example`

### Inspect containers using Go template engine

`docker inspect -f '{{.State.ExitCode}}' jboss.wildfly`

### Fetch the IP address from the metadata of the container

`docker inspect rest-example | jq -r '.[0].NetworkSettings.IPAddress'`

### Get stats of all (not only running) the containers

`docker stats -a`

### Fetch container events

`docker events`

### Restart a container after it is shut down (in every case)

`docker run --restart=always rest-example`

### Restart a container after it is shut down (with a non-zero exit status)

`docker run --restart=on-failure:5 rest-example`

### Get the number of restarts of a container

`docker inspect -f "{{ .RestartCount }}" rest-example`

### Discover the last time the container was started again

`docker inspect -f "{{ .State.StartedAt }}" rest-example`

### Updating a restart policy on a running container

`docker run -d -t rest-example`

`docker update --restart=always rest-example`

### Restrict the memory to 512 MB for the container

`docker run -it -m 512m ubuntu`

### Set memory reservation without setting the hard memory limit

`docker run -it --memory-reservation 1G ubuntu /bin/bash`

### Set the Kernel memory limit

`docker run -it --kernel-memory 100M ubuntu /bin/bash`

### Setting user memory and kernel memory for a container

`docker run -it -m 1G --kernel-memory 100M ubuntu /bin/bash`

### Set memory swappiness constraint for a container (0 to disable swaps, 100 all anonymous pages to swap out)

`docker run -it --memory-swappiness=0 ubuntu /bin/bash`

### Set the CPU usage limit for a container to 50% of the CPU resource

`docker run -it --cpu-quota=50000 ubuntu /bin/bash`

### Constraint the container to get 50% of the CPU usage every 50ms

`docker run -it --cpu-quota=25000 --cpu-period=50000 ubuntu /bin/bash`

### Set the number of CPU cores to be used by the container

`docker run -it --cpuset 2 ubuntu`

### Updating constrains of a running container

`docker update --cpu-shares 512 -m 500M rest-example`

# Java Microservices

### Using Maven to generate Docker images (without Dockerfile)

`cd dokuja`

`mvn clean package docker:build`

### Using Maven to start a container

`mvn clean package docker:start`

Or to stop the container

`mvn docker:stop`

### Using Maven to run a container

`mvn clean package docker:run`


