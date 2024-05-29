FROM postgres:14.1-alpine

ENV POSTGRES_DB=db \
   POSTGRES_USER=usr 

COPY ./Data/InsertData.sql/ /docker-entrypoint-initdb.d/
COPY ./Data/CreateScheme.sql/ /docker-entrypoint-initdb.d/

