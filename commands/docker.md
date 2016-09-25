### docker-machine
*	**ls**  
 		lists available (running & non-running) docker machines (virtual machines created by docker-machine)
*	**create --driver virtualbox _myvm_**  
		downloads a lightweight Linux distribution (boot2docker) with the Docker daemon installed  
		creates a machines with the name myvm using virtualbox as a driver  
*	**create --driver virtualbox --engine-insecure-registry _pvtdockerrepo:5000_ _myvm_**  
		downloads a lightweight Linux distribution (boot2docker) with the Docker daemon installed  
		creates a machines with the name myvm using virtualbox as a driver  
		configures insecure connection to pvtdockerrepo docker registry  
*	**ip _myvm_**  
		gets the ip of the virtual machine
*	**active**  
		shows active virtual machine
*	**stop _myvm_**  
		stops the virtual machine
*	**inspect _myvm_**  
		shows the details of the virtual machine
*	**rm _myvm_**  
		removes the virtual machine
*	**@FOR /f "tokens=*" %i IN ('docker-machine env --shell cmd _myvm_') DO @%i**  
		connects to myvm machine (env variables need to be set) and makes it active machine (on cmd.exe)
*	**env _myvm_ --shell powershell | Invoke-Expression**  
		connects to myvm machine (env variables need to be set) and make it active machine (on PowerShell)
*	**start _myvm_**  
		starts the machine
*	**stop _myvm_**  
		stops the machine
*	**regenerate-certs _myvm_**  
		regenerates certificates  
		it's required when there is ip mismatch  
		IP might change at VM startup/restart

### docker
*	**version**  
		docker server and client version
*	**ps**  
		list the running containers (e.g. MySql)
*	**ps -a**  
		list all the running and non-running containers
*	**run _imagename_**  
		start a container for the image
*	**run -d _imagename_**  
		start a container for the image in daemon mode
*	**run -d -p _hostport_:_containerport_ _imagename_**  
		start a container for the image  in daemon mode and map the port to host port
*	**pull _pvtdockerrepo:5000_/_myapp:latest_**  
		pull image from pvtdockerrepo registry
*	**images**  
		show list of available images
*	**build -t _imagename_ .**  
		creates an image with name imagename using docker file in the current folder  
		Note: only the folders required for image should be present in the folder  
		, where Dockerfile is present. Otherwise, build tries to compress complete parent folder.  
*	**build -t _pvtdockerrepo:5000_/_imagename:latest_**  
		creates an image with name imagename (and tags it with repo name) using docker file in the current folder
*	**docker tag _imagename:latest_ _pvtdockerrepo:5000_/_imagename:latest_**  
		tag image
*	**docker push _pvtdockerrepo:5000_/_imagename:latest_**  
		push image to a private docker registry
*	**exec -it _containername_ /bin/bash**  
		get inside running container 
*	**logs _containername_**  
		view the running logs of the container
*	**logs _containername_ -f --tail 10**  
		view the running logs of the container
		keep following logs
		start followings logs from last 10 lines of the logs
*	**kill _containername_**  
		kill the docker container with name containername
*	**rm _containerid_**  
		remove the container with the mentioned containerid
*	**rmi _imageid_**  
		remove the image with the mentioned imageid
*	**stats --all**  
		show the live stream statistics (CPU, memory, IO, network etc.) of all the running containers 

### docker-compose
*	**-f _docker-compose-yml-file-path_ up -d**  
	brings container/s up based on the configuration in the file at path docker-compose-yml-file-path
