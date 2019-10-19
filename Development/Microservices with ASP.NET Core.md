# Microservices with ASP.NET Core

Saturday, October 19, 2019
5:34 AM

## Install Postman

```Shell
sudo snap install postman
```

```Shell
clear

sudo snap switch --channel=candidate postman
sudo snap refresh postman
```

### References

**How to Install Postman on Ubuntu 18.04**\
From <[https://linuxize.com/post/how-to-install-postman-on-ubuntu-18-04/](https://linuxize.com/post/how-to-install-postman-on-ubuntu-18-04/)>

**Stuck in an Installation loop on Ubuntu 18.04**\
From <[https://community.getpostman.com/t/stuck-in-an-installation-loop-on-ubuntu-18-04/4832](https://community.getpostman.com/t/stuck-in-an-installation-loop-on-ubuntu-18-04/4832)>

Docker

docker container stop \$(docker container ls -aq)

docker container rm \$(docker container ls -aq)

# Delete all Docker containers\
# Must be run first because images are attached to containers\
docker rm -f \$(docker ps -a -q)

# Delete every Docker image\
docker rmi -f \$(docker images -q)


