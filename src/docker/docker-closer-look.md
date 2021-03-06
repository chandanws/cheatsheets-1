## Closer Look

### Docker Engine ###

	/var/lib/docker/containers
	<full-container-id>
	└── 1ee8a5d778cba1738b636183a1e2470fe2699af3a127f50cd012f08dcd47cf4e-json.log
	    ├── config.json
	    ├── hostconfig.json
	    ├── hostname
	    ├── hosts
	    └── resolv.conf

A full-container-id is the real name of a container.
It is shortened to the first 12 chars when reported by tools. i.e.

	full-container-id: 1ee8a5d778cba1738b636183a1e2470fe2699af3a127f50cd012f08dcd47cf4e
	     container-id: 1ee8a5d778cb<------------------ removed ----------------------->

	/var/lib/docker/vfs/dir
	├── <volume-id>
	├── <volume-id>
	├── <volume-id>
	└── <volume-id>

### Images ###

* Docker images are the basis of containers
* If an image isn't already present on the host then it'll be downloaded from a registry
	* by default the Docker Hub Registry
* Docker stores downloaded images on the Docker host

* Creating new images
	* workflow #1: Start from existing image
	* workflow #2: Define a new image through a Dockerfile

### Networking ###

By default, every time a container is restarted, it binds to different host ports.

### Container Linking ###

![Linking Example](docker-linked-containers.png "Linking Example")

Docker creates these env variables in the "source" container for each "linked" container.

	<alias>_NAME=/<this_name>/<alias>
	<alias>_PORT_<exposed_port>_<protocol>=<protocol>://<linked_container_ip_address>:<port>
	<alias>_PORT_<exposed_port>_<protocol>_ADDR=<linked_container_ip_address>
	<alias>_PORT_<exposed_port>_<protocol>_PORT=<actual_port>
	<alias>_PORT_<exposed_port>_<protocol>_PROTO=<protocol>

/etc/hosts is also changed

	<this_container_ip>       <this_container_id>
	<linked_container_ip>     <linked_container_alias>

### Volumes ###

* Primary ways you can manage data in Docker
	* Data volumes
		* A specially-designated directory within one or more containers that bypasses the Union File System 
		* to provide several useful features for persistent or shared data:
			* Data volumes can be shared and reused between containers
			* Changes to a data volume are made directly
			* Changes to a data volume will not be included when you update an image
			* Volumes persist until no containers use them
		* Can be...
			* a new directory in the container
			* mapped to a directory in the host
			* mapped to a file in the host
	* Data volume containers

* Volumes 
	* decouple 
		* the life of the data being stored in them 
		* from the life of the container that created them. 
	* so you can 
		* docker rm my_container and your data will not be removed.

* A volume can be created in 
	* two ways
		* Specifying VOLUME /some/dir in a Dockerfile
		* Specying it as part of your run command as docker run -v /some/dir
	* Either way, 
		* It tells Docker to create a directory on the host, 
		* within the docker root path (by default /var/lib/docker), 
		* and mount it to the path you've specified (/some/dir above). 
	* When you remove the container using this volume, the volume itself continues to live on.
