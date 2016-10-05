## Handy commands on _minikube_ and _kubectl_ for starters  
#### Make sure to open the windows command in administration mode to run minikube commands.  
#### These commands are specific to Windows cmd
#### Docker runs directly once we set docenv; but on VPN, we might have to use minikube ssh docker to run it. Another scenario, where docker commands might not run directly is when the dvm version is different. Check the dvm version of the docker (docker ls) and use the right one (e.g. docker use 1.11.1)
### minikube  
*	**start**  
		starts a VM  
*	**start --docker-env HTTPS_PROXY=https://proxy_user_name:proxy_user_pwd@proxy_server_id:proxy_server_port --docker-env HTTP_PROXY=http://proxy_user_name:proxy_user_pwd@proxy_server_id:proxy_server_port --insecure-registry=pvtdockerrepo:5000 --docker-env NO_PROXY=pvtdockerrepo --logtostderr --show-libmachine-logs**  
		Start minikube  
		set enterprise proxy for docker env so that it can download images from internet if someone is behind proxy  
		register private docker images registry pvtdockerrepo  
		set no-proxy for private docker registory so that docker connects to pvtdockerrepo registry without proxy  
*	**status**  
		status of VM  
*	**ip**  
		Get Minikube Cluster IP
*	**dashboard**  
		Open minikube dashboard
*	**stop**  
		Stop Minikube Cluster
*	**delete**  
		delete minikubeVM
*	**ssh <docker command>**  
		to run docker commands inside minikubeVM  
		or set docker env using below command to use docker directly
*	**@FOR /f "tokens=*" %i IN ('minikube docker-env') DO @%i**  
		Set the docker to point to VM created by minikube (in cmd.exe). Don't pre-fix this command with minikube

---
### kubectl
*	**get nodes**  
		Get Nodes
*	**delete node NODE_NAME**  
		Delete Node. _stop_ as well is used for it; but _stop_ is deprecated.
*	**get deployments**  
		Check your deployment (or use get deployment)
*	**delete deployment DEPLOYMENT_NAME**  
		Delete deployment
*	**get pods**  
		Check the PODS
*	**get svc**  
		Check Sevices
*	**get cs**  
		Health Check
*	**run _mydeploymentname_ --image=_pvtdockerrepo:5000_/_imagename_ --port=proxy_server_port**  
		creates deployment and pod with container of _imagename_ image
*	**expose deployment mydeploymentname --target-port=proxy_server_port --type=NodePort**  
		Expose the POD port to outside
*	**create -f _k8sconfigfile_**  
		creates configuration for pods/deployments/services using _k8sconfigfile_   
*	**create -f _k8sconfigfiledirectory_**  
		creates configuration for pods/deployments/services using on yaml/yml/files in _k8sconfigfiledirectory_   
*	**apply -f _k8sconfigfile_**  
		creates or updates configuration for pods/deployments/services using _k8sconfigfile_   
*	**apply -f _k8sconfigfiledirectory_**  
		creates or updates configuration for pods/deployments/services using on yaml/yml/files in _k8sconfigfiledirectory_   
*	**describe svc mydeploymentname**  
		view dertails of the mydeploymentname service.  
		Note down NodePort to access application using the minikube IP and the node port  
