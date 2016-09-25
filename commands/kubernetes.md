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

### kubectl
*	**get nodes**  
		Get Nodes
*	**delete node NODE_NAME**  
		Delete Node
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
*	**run mydeploymentname --image=pvtdockerrepo:5000/imagename --port=proxy_server_port**  
		creates an instance of imagename image in (a new pod or existing pod if it's second time?)  
		YML could be provided as well. what's the exact command for this?  
*	**expose deployment mydeploymentname --target-port=proxy_server_port --type=NodePort**  
		Expose the POD port to outside
*	**describe svc mydeploymentname**  
		view dertails of the mydeploymentname service.  
		Note down NodePort to access application using the minikube IP and the node port  
