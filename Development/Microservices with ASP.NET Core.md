# Microservices with ASP.NET Core

Saturday, October 19, 2019\
5:34 AM

## Docker

```Console
docker container stop $(docker container ls -aq)

docker container rm $(docker container ls -aq)

# Delete all Docker containers
# Must be run first because images are attached to containers
docker rm -f $(docker ps -a -q)

# Delete every Docker image
docker rmi -f $(docker images -q)
```
