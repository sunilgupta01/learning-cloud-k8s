### docker-machine
	ls
		list of avilable (running) machines (virtualboxes)
	create --driver virtualbox myvm
		creates a machines with the myvm using virtualbox as a driver
		This command downloads a lightweight Linux distribution (boot2docker) with the Docker daemon installed
		, and creates and starts a VirtualBox VM with Docker running.
	create --driver virtualbox --engine-insecure-registry pvtdockerrepo:5000 newmyvm
		creates machine with insecure connection to mentioned registry
		This is for connecting to registry on pvtdockerrepo that is currently insecure.
	ip myvm
		get the ip of virutal machine name
	active
		shows active virtual machine....what does it mean?????
	stop myvm
		stops the docker machine
	inspect myvm
		shows the complete list of deatils of the machine
	rm myvm
		removes the machine
	@FOR /f "tokens=*" %i IN ('docker-machine env --shell cmd myvm') DO @%i
		connect to myvm machine (env variables need to be set) and make it active machine (on cmd.exe)
	env myvm --shell powershell | Invoke-Expression	
		connect to myvm machine (env variables need to be set) and make it active machine (on powershell)
	start myvm
		start the machine
	stop myvm
		stop the machine
	regenerate-certs myvm
		regenerate certificates.
		it's required when there is ip mismatch
		IP might change at VM startup/restart
		
### docker
	version
		docker server and client version
	ps
		list the running containers (e.g. mysql)
	ps -a
		list all the running and non-running containers
	run imagename
		start a contianer for the image
	run -d imagename
		start a contianer for the image in daemon mode
	run -d -p hostport:containerport imagename
		start a contianer for the image  in daemon mode and map the port to host port
	pull pvtdockerrepo:5000/myapp:latest
		pull image from pvtdockerrepo registry
	images
		show list of available images
	build -t imagename .
		creates an image with name imagename using docker file in the current folder
		Note: only the folders required for image should be present in the folder
		, where the dockerfile is present. Otherwise, build tries to compress complete parent folder.
	build -t pvtdockerrepo:5000/imagename:latest
		creates an image with name imagename (and tags it with repo name) using docker file in the current folder
	docker tag imagename:latest localhost:5000/imagename:latest
		tag image
	docker push localhost:5000/imagename:latest
		push image to a private docker registry
	exec -it containername /bin/bash
		get inside running container 
	logs containername
		view the running logs of the container
	logs containername -f --tail 10
		view the running logs of the container
		keep following logs
		start followings logs from last 10 lines of the logs
	kill containername
		kill the docker container with name containername
	rm containerid
		remove the container with the mentioned containerid
	rmi imageid
		remove the image with the mentioned imageid
	stats --all
		show the live stream statistics (CPU, memory, IO, network etc.) of all the running containers 

### docker-compose
	-f docker-composeymlfilepath up -d
