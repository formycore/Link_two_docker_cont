# original https://github.com/vikash-kumar01/mrdevops-springboot-mongo-docker-application.git
# Cloned it to https://github.com/formycore/Link_two_docker_cont.git
- Link two docker containers
### Pull Mongo db  
```docker pull mongo```
### Check the docker images
```docker images ls```
```docker images```
### Run the container
```docker run -d --name mongodblink -p 27017:27017 mongo:latest
# this name mongodblink should be same as src > main > resources > application.yml > host 
```

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


