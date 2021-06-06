
[Reference Documentation](https://www.jenkins.io/doc/book/installing/docker/)

1. Create a network
```
docker network create jenkins
```

2. Create
```
docker run \
  --name jenkins-docker \
  --rm \
  --detach \
  --privileged \
  --network jenkins \
  --network-alias docker \
  --env DOCKER_TLS_CERTDIR=/certs \
  --volume jenkins-docker-certs:/certs/client \
  --volume jenkins-data:/var/jenkins_home \
  --publish 2376:8080 \
  jenkins/jenkins:lts
```
lts branch - long term support
3. note down password from container logs
