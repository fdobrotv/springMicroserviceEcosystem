# Getting Started (docker-compose application run)
Spring Cloud ecosystem demo bootstrap project

## Requirements
Java 15, Gradle 6.6+

Docker, docker-compose

### Host name aliases in hosts file
cat `127.0.0.1 discovery-service.com
127.0.0.1 discovery-service-1.com
127.0.0.1 discovery-service-2.com
127.0.0.1 discovery-service-3.com
127.0.0.1 discovery-service-4.com

127.0.0.1 gateway-service.com
127.0.0.1 gateway-service-1.com

127.0.0.1 configuration-service.com
127.0.0.1 configuration-service-1.com

127.0.0.1 client-service.com
127.0.0.1 client-service-1.com
127.0.0.1 client-service-2.com
127.0.0.1 client-service-3.com

127.0.0.1 tracing-service-1.com

127.0.0.1 db.com
127.0.0.1 db-1.com
127.0.0.1 db-2.com
127.0.0.1 db-3.com

127.0.0.1 message-broker-service
127.0.0.1 zookeeper-service` >> /etc/hosts

## Prepare environment to run
`docker-compose up`

### Maybe you also need to initialize the cluster
`docker exec -it db-1.com ./cockroach init --insecure`

## Useful docker-compose commands
Exclude some service from start
`docker-compose up --scale configuration-service-1.com=0`

#Helm (Kubernetes application run)
## Helm repositories
`helm repo add bitnami https://charts.bitnami.com/bitnami`
`helm repo add jaeger-all-in-one https://raw.githubusercontent.com/hansehe/jaeger-all-in-one/master/helm/charts`
`helm repo add cockroachdb https://charts.cockroachdb.com`
## Pull helm repositories
`helm dependency update charts/all-service`

## Minikube start
1) install kubectl and minikube
2) `minikube config set driver docker` or hyperkit or virtualbox
2.1 optional) `minikube config set memory 8192`
2.2 optional) `minikube config set cpus 4`
3) `minikube start --container-runtime=cri-o` for local development better use docker instead of cri-o because you can do `eval $(minikube docker-env)`
4) install helm

## Minikube dashboard
`minikube dashboard`

## run application with helm
`helm install all-service charts/all-service`

## update application with helm
`helm upgrade all-service charts/all-service`

## Useful k8s commands
### push image to k8s docker registry (for local development)
1) re-use the Docker daemon inside the Minikube instance `eval $(minikube docker-env)`
2) build image (`gradlew jibDockerBuild`)
3) check `docker images`

### get host machine ip from minikube (docker runtime)
`minikube ssh grep host.minikube.internal /etc/hosts | cut -f1`

### ClusterIP port forwarding (from k8s cluster to local port)
https://kubernetes.io/docs/tasks/access-application-cluster/port-forward-access-application-cluster/
example: `kubectl port-forward service/configuration-service 7000:8888`

### install helm plugin for IDEA (recommendation)

### Used project links
https://github.com/fdobrotv/springDiscoveryService
https://github.com/fdobrotv/springConfigurationService
https://github.com/fdobrotv/springCloudGateway
https://github.com/fdobrotv/springCloudClientService
https://github.com/fdobrotv/cloud_config