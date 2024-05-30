
---

# DevOps Project README

## Overview

This project consists of three main components:

1. HTTP server
2. Database
3. Backend API

---

## TP1

### Database Part

Dockerfile Creation :

We start by creating a Dockerfile for the database component:

```Dockerfile
FROM postgres:14.1-alpine

ENV POSTGRES_DB=db \
   POSTGRES_USER=usr 

COPY ./InsertData.sql/ /docker-entrypoint-initdb.d/
COPY ./CreateScheme.sql/ /docker-entrypoint-initdb.d/
```

### Building the Docker Image

```bash
docker build -t comerichard/postgres_image .
```

### Network Creation

```bash
docker network create app-network2
```

### Running the Database Container with its volume

```bash
docker run --name postgres_devops --network app-network2 -e POSTGRES_PASSWORD=pswd -p 5432:5432 \
-v C:/Users/richa/Documents/EPF/MDE/DevOps/Data:/Data/ -d comerichard/postgres_image
```

### Running the Adminer Container

```bash
docker run --name adminer-container --network app-network2 -p 8090:8080 adminer
```

---

## Backend API

### Building and Running the Backend API Container

We create the image and run the container of simple-api, including the creation of main.java:

```bash
docker build -t java-hello-world_4 .
docker run -p 8091:8080 --network app-network2 java-hello-world_4
```

This will display "Hello World" at the localhost.

### Multi-stage Dockerfile Explanation

Multi-stage Dockerfile allows us to first build the image from Maven and then automatically run myapp. The build stage enables us to copy directories and run Maven commands. The run stage sets the environment and specifies the default command to run when a container is created.

---

## Http server

### Reverse Proxy 

First the creation of the index.html to display something on localhost. Then the changement of the configuration to set the container and its ports to communicate.
A reverse proxy will allow to separate user from the backend. The users will first communicate with proxy and get what is returned.

---

## Link application

### Docker-compose

```bash
docker-compose up
```

Docker-compose up will allow us to run all the containers with only one command. It diminuish the probability of making errors and use less time.

---

## Publish

### Dockerhub


```bash
docker tag database comerichard/database:1.0
docker push comerichard/database:1.0  
```

This is done for the three containers so taht we tag them and publish them on dockerhub. Someone can now download our images.

---

--- 

## TP2: Continuous Integration (CI)

For Windows, Maven needs to be downloaded from WSL, making it easier to use. Ensure that the Java version is correct; it will work on GitHub machines if the version is appropriate.

### Setting up CI / CD

Create the `main.yml` file and define the necessary jobs. With this configuration, tests will be automatically launched every time you push changes.

pom.xml is a configuration file used by maven, testcontainers allow us to use containers in our automated tests which is what we want for automatisation.

Creation of the .github/workflow repo where the tests will be writen in a yml file.

### Continious Delivery

This part is to build and publish new images on dockerhub every time we commit. This way you do not have to do it manually and every thing is automated when a commit is done.

### Quality Gate

Connected on sonarcloud, it will test our application automatically. 

We have encountered 2 issues, achieved 53.6% test coverage, and failed 3 security hotspots. So Dev have some work to do !

---

---

## TP3: Using Roles in Ansible

The purpose is to deploy the app on the server.

### Setup.yml

The setup.yml is here to give the host and access to our server.

```bash
ansible all -i inventories/setup.yml -m ping
```
We can try to ping the server to verify.

### Playbooks

The playbooks is a config file of ansible, it has all the tasks and allow automatisation.

### Roles

Roles in Ansible allow for better organization and modularity. We have a `.yml` file that redirects to tasks stored in roles/docker/task. These tasks are responsible for managing Docker.

### Deploy the app

The deployment is made in differents task. We use a principal playbook that will call different yml file. These file will automatically download in our server Docker, python, it launch the db, the app and the proxy. Globally it do everything automatically.

### Continuous Deployment

We create  a deploy.yml in our workflow, that deploy the app.

---
