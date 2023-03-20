# original https://github.com/vikash-kumar01/mrdevops-springboot-mongo-docker-application.git
# Cloned it to 
- Link two docker containers
### Pull Mongo db  
```docker pull mongo```
### Check the docker images
```docker images ls```
```docker images```
### Run the container
```docker run -d --name mongodblink -p 27017:27017 mongo:latest```

### Now Dockerize our application 