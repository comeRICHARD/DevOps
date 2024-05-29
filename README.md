**TP1**

**This is a 3 parts applications :**
- HTTP server
- Database
- Backend API

**DataBase Part**

*First we create the dockerfile* 
FROM postgres:14.1-alpine

ENV POSTGRES_DB=db \
   POSTGRES_USER=usr 

COPY ./InsertData.sql/ /docker-entrypoint-initdb.d/
COPY ./CreateScheme.sql/ /docker-entrypoint-initdb.d/

*Then build the image*
docker build -t comerichard/postgres_image .

*We create the network*
docker network create app-network2

*We run the container with its volume*
docker run --name postgres_devops --network app-network2 -e POSTGRES_PASSWORD=pswd -p 5432:5432 
-v C:/Users/richa/Documents/EPF/MDE/DevOps/Data:/Data/ -d comerichard/postgres_image

*We run the adminer container*
docker run --name adminer-container --network app-network2 -p 8090:8080 adminer


**BackEnd API**
*We create the image and run container of simple-api, creation of main.java*
docker build -t java-hello-world_4 .
docker run -p 8091:8080 --network app-network2 java-hello-world_4
This will show hello world at the localhost.

Multi stage allows us to first build the image from maven and then run automatically myapp. The build stage allows us to copy some directories and run some maven’s commands.
The run stage set the environment and specify the default command to run when a container is created.

**TP2**

*CI*
For windows mvn needs to be downloaded from wsl for example it is far more easy.
Then if the java version is not the good one pass and it will work on github machines.

Create the main.yml and write the jobs you need. Every time you push it will launch tests automically.

*Set up a quality gate*



*Quality Gate* 

We have 2 issues, 53,6% of coverage, 3 security hotspots failed.

**TP3**
*Using Roles*
So we have in ansible a .yml which redirect to the tasks which are in roles/docker/task. The task are responsible for the downloading of docker.