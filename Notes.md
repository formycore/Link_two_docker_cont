# original https://github.com/vikash-kumar01/mrdevops-springboot-mongo-docker-application.git
# Cloned it to https://github.com/formycore/Link_two_docker_cont.git
- Link two docker containers
### Pull Mongo db  
```docker pull mongo```
### Check the docker images
```docker images ls```
```docker images```
### Run the container
```docker run -d --name mongodblink -p 27017:27017 mongo:latest```
```this name mongodblink should be same as src > main > resources > application.yml > host```


### Now Dockerize our application
```
# DONT CHANGE THE JAR FILE 
git clone https://github.com/formycore/Link_two_docker_cont.git
vi Dockerfile

FROM maven as build
WORKDIR /app
COPY . .
RUN mvn install

FROM openjdk:8
WORKDIR /app
COPY --from=build /app/target/springboot-mongo-docker.jar /app/springboot-mongo-docker.jar
EXPOSE 8080
ENTRYPOINT ["java","-jar","springboot-mongo-docker.jar"]

```
### Run the docker build
```docker build -t springboot-mongodb:1.0 .```

### Check the docker containers
```docker ps```

### Now run our container
```
docker run -p 8080:8080 --name <any name> --link <running_db_container_name>:<docker_image_of_this_container> -d <docker image>

docker run -p 8080:8080 --name springboot-mongodb --link mongodblink:mongo -d springboot-mongodb:1.0
```
### with postman we can get,post the info to the database server 
- select get
- enter the URL:8080/book
- select body
- select raw
- select json
- select post 
- in the bar type 
{
        "id": 1,
        "name": "Think and Grow Rich",
        "authorName": "Nepolian Hill"
    },
     {
        "id": 2,
        "name": "Do Epic Shit",
        "authorName": "Ankur Warikoo"
    }
- now select get (click on send)
#### now checking from inside the db container
```
docker exec -it <db_container_name> /bin/bash
# Inside the container
mongosh
show dbs
use Book
show collections
# to view all the books insie the container
db.books.find().pretty()
```
# Now with docker-compose
```
version: "3"
services:
  mongodblink: # this is one service 
    image: mongo:latest # this is image needs to pull
    container_name: "mongodblink" # this container name
    ports:
      - 27017:27017
  springboot-mongodb: # this is another service
    image: springboot-mongodb:1.0 # this image is already there in the system
    container_name: springboot-mongodb # this is container name
    ports:
      - 8080:8080
    links: # this is for linking 
      - mongodblink
```
