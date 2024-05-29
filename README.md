
---

# DevOps Project README

## Overview

This project consists of three main components:

1. HTTP server
2. Database
3. Backend API

---

## Database Part

### Dockerfile Creation

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

### Running the Database Container

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

## TP2: Continuous Integration (CI)

For Windows, Maven needs to be downloaded from WSL, making it easier to use. Ensure that the Java version is correct; it will work on GitHub machines if the version is appropriate.

### Setting up CI

Create the `main.yml` file and define the necessary jobs. With this configuration, tests will be automatically launched every time you push changes.

### Quality Gate

We have encountered 2 issues, achieved 53.6% test coverage, and failed 3 security hotspots.

---

## TP3: Using Roles in Ansible

Roles in Ansible allow for better organization and modularity. We have a `.yml` file that redirects to tasks stored in roles/docker/task. These tasks are responsible for managing Docker.

---

This README provides an overview of the project structure, setup instructions, and insights into continuous integration and quality control processes. For more detailed information, refer to individual sections and files within the project.