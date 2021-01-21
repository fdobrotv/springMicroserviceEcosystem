# Getting Started
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

127.0.0.1 vault-1.com

127.0.0.1 message-broker-service
127.0.0.1 zookeeper-service` >> /etc/hosts

## Prepare environment to run
`docker-compose up`

### Maybe you also need to initialize the cluster
`docker exec -it db-1.com ./cockroach init --insecure`

## Helm repositories
`helm repo add bitnami https://charts.bitnami.com/bitnami`
`helm repo add jaeger-all-in-one https://raw.githubusercontent.com/hansehe/jaeger-all-in-one/master/helm/charts`
`helm repo add cockroachdb https://charts.cockroachdb.com`
### Pull helm repositories
`helm dependency update charts/all-service`

## Minikube dashboard
`minikube dashboard`

## K8s run
`helm install all-service charts/all-service`

## K8s chart development
`helm upgrade all-service charts/all-service`

## Kubernetes installation with 1 node (minikube)
1) install hypervisor (virtualbox or hyperkit)
2) install minikube
3) install kubectl
4) `minikube config set driver docker` or hyperkit or virtualbox
4.1 optional) `minikube config set memory 8192`
4.2 optional) `minikube config set cpus 4`
5) `minikube start --container-runtime=cri-o` for local development better use docker instead of cri-o because you can do `eval $(minikube docker-env)`
6) install helm

## Deploy all services using k8s and helm
1) run `kubectl apply -f ./fabric8-rbac.yaml`

[comment]: <> (2&#41; start database and vault services using docker `docker-compose up -d db-1.com vault-config`)
3) start tracing service: run `kubectl apply -f ./tracing-service-service.yaml -f ./tracing-service-deployment.yaml`
4) start all services with single helm chart: go inside main-service chart directory and run `helm install main-service .`
5.0) OR start all services separately:
5.1) start configuration service: go inside configuration-service chart directory and run `helm install configuration-service .`
5.2) start client service: go inside client-service chart directory and run `helm install client-service .`
5.3) start gateway service: go inside gateway-service chart directory and run `helm install gateway-service .`

## Useful docker-compose commands
Exclude some service from start
`docker-compose up --scale configuration-service-1.com=0`

## Useful k8s commands
### push image to k8s docker registry (for local development)
1) re-use the Docker daemon inside the Minikube instance `eval $(minikube docker-env)`
2) build image (`gradlew jibDockerBuild`)
3) check `docker images`
4) when you define k8s deployment yaml, do not forget to set `imagePullPolicy: Never`

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