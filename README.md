Spring Boot Microservices
This repository contains the latest source code of the spring-boot-microservices tutorial

You can watch the tutorial on Youtube here

How to launch this tutorial?  
1) kubectl port-forward svc/api-gateway 9000:9000 --> start backend servis
2) kubectl port-forward svc/keycloak 8080:8080 --> start Login, authorization servis
3) kubectl port-forward svc/frontend 4200:80 --> start frontend servis
4) kubectl port-forward svc/grafana 3000:3000 --> start logs, metrics servis, to track how our application, backend is working

Services Overview
Product Service
Order Service
Inventory Service
Notification Service
API Gateway using Spring Cloud Gateway MVC
Shop Frontend using Angular 18
Tech Stack
The technologies used in this project are:

Spring Boot
Angular
Mongo DB
MySQL
Kafka
Keycloak
Test Containers with Wiremock
Grafana Stack (Prometheus, Grafana, Loki and Tempo)
API Gateway using Spring Cloud Gateway MVC
Kubernetes
Application Architecture
image

How to run the frontend application
Make sure you have the following installed on your machine:

Node.js
NPM
Angular CLI
Run the following commands to start the frontend application

cd frontend
npm install
npm run start
How to build the backend services
Run the following command to build and package the backend services into a docker container

mvn spring-boot:build-image -DdockerPassword=<your-docker-account-password>
The above command will build and package the services into a docker container and push it to your docker hub account.

How to run the backend services
Make sure you have the following installed on your machine:

Java 21
Docker
Kind Cluster - https://kind.sigs.k8s.io/docs/user/quick-start/#installation
Start Kind Cluster
Run the k8s/kind/create-kind-cluster.sh script to create the kind Kubernetes cluster

./k8s/kind/create-kind-cluster.sh
This will create a kind cluster and pre-load all the required docker images into the cluster, this will save you time downloading the images when you deploy the application.

Deploy the infrastructure
Run the k8s/manisfests/infrastructure.yaml file to deploy the infrastructure

kubectl apply -f k8s/manifests/infrastructure.yaml
Deploy the services
Run the k8s/manifests/applications.yaml file to deploy the services

kubectl apply -f k8s/manifests/applications.yaml
Access the API Gateway
To access the API Gateway, you need to port-forward the gateway service to your local machine

kubectl port-forward svc/gateway-service 9000:9000
Access the Keycloak Admin Console
To access the Keycloak admin console, you need to port-forward the keycloak service to your local machine

kubectl port-forward svc/keycloak 8080:8080
Access the Grafana Dashboards
To access the Grafana dashboards, you need to port-forward the grafana service to your local machine

kubectl port-forward svc/grafana 3000:3000
