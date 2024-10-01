 
#  How to launch this tutorial?  
1) kubectl port-forward svc/api-gateway 9000:9000 --> start backend servis
2) kubectl port-forward svc/keycloak 8080:8080 --> start Login, authorization servis
3) kubectl port-forward svc/frontend 4200:80 --> start frontend servis
4) kubectl port-forward svc/grafana 3000:3000 --> start logs, metrics servis, to track how our application, backend is working

# Spring Boot Microservices
This repository contains the latest source code of the spring-boot-microservices tutorial

You can watch the tutorial on Youtube [here](https://youtu.be/yn_stY3HCr8?si=EjrBEUl0P-bzSWRG)

## Services Overview

- Product Service
- Order Service
- Inventory Service
- Notification Service
- API Gateway using Spring Cloud Gateway MVC
- Shop Frontend using Angular 18

## Tech Stack

The technologies used in this project are:

- Spring Boot
- Angular
- Mongo DB
- MySQL
- Kafka
- Keycloak
- Test Containers with Wiremock
- Grafana Stack (Prometheus, Grafana, Loki and Tempo)
- API Gateway using Spring Cloud Gateway MVC
- Kubernetes


## Application Architecture
![image](https://github.com/user-attachments/assets/d4ef38bd-8ae5-4cc7-9ac5-7a8e5ec3c969)

## How to run the frontend application

Make sure you have the following installed on your machine:

- Node.js
- NPM
- Angular CLI

Run the following commands to start the frontend application

```shell
cd frontend
npm install
npm run start
```
## How to build the backend services

Run the following command to build and package the backend services into a docker container

```shell
mvn spring-boot:build-image -DdockerPassword=<your-docker-account-password>
```

The above command will build and package the services into a docker container and push it to your docker hub account.

## How to run the backend services

Make sure you have the following installed on your machine:

- Java 21
- Docker
- Kind Cluster - https://kind.sigs.k8s.io/docs/user/quick-start/#installation

### Start Kind Cluster
    
Run the k8s/kind/create-kind-cluster.sh script to create the kind Kubernetes cluster

```shell
./k8s/kind/create-kind-cluster.sh
```
This will create a kind cluster and pre-load all the required docker images into the cluster, this will save you time downloading the images when you deploy the application.

### Deploy the infrastructure

Run the k8s/manisfests/infrastructure.yaml file to deploy the infrastructure

```shell
kubectl apply -f k8s/manifests/infrastructure.yaml
```

### Deploy the services

Run the k8s/manifests/applications.yaml file to deploy the services

```shell
kubectl apply -f k8s/manifests/applications.yaml
```

### Access the API Gateway

To access the API Gateway, you need to port-forward the gateway service to your local machine

```shell
kubectl port-forward svc/gateway-service 9000:9000
```

### Access the Keycloak Admin Console
To access the Keycloak admin console, you need to port-forward the keycloak service to your local machine

```shell
kubectl port-forward svc/keycloak 8080:8080
```

### Access the Grafana Dashboards
To access the Grafana dashboards, you need to port-forward the grafana service to your local machine

```shell
kubectl port-forward svc/grafana 3000:3000
``` 

# Question:
please help i dont understand how the services are protected with such security as shown in the video as only the gateway will be protected but if we use services port directly then there are not protected

# Answer: 
The services inside Kubernetes cannot be accessed from outside as they are using ClusterIP, so it's fine if we just secure the API gateway which is the entry point of the system.

Securing each and every service can also be done but that will complicate the project without any additional learnings, that's why I chose to go with this Approach

# Question:
could you please explain why in this video you defined route class for implementing API Gateway, but another video implement it via application properties? for which 
purpose each one is used?

# Answer:
I am using Spring Cloud Gateway MVC in this video, there are some configurations missing in the MVC variant that's why I had to use the Java Config in this video.

The previous tutorial I was using Spring Cloud Gateway that is based on Spring We flux, this project is more mature and provides all configuration through properties config.

# Question: 
I have followed several courses on microservices and I have seen that they define a microservice with the name config-server to centralize the configuration of the other microservices and also define another microservice for eureka server. 

Could you explain me why you didn't make use of them? or in which cases they should be used and when not ?

# Answer:
Eureka Server and Config Server can be used when you are not using Kubernetes.

But when you are using Kubernetes both those features are not useful anymore because Kubernetes provides us service discovery out if the box and also it supports centralized configuration through Config Maps.

So there is no need to do that for our project.
