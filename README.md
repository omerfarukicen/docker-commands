
# docker-commands

## Create User
   
    sudo usermod -aG docker $USER
    newgrp docker

## Create docker

    docker create hello-world

### Start docker

    docker start -a "dockerID"
    docker run    "imageName"
    docker start  "containerId"
**Note:**  Docker Run is going to show you all the logs or al the information Docker start opposite but quickly

###  Stop 

    docker system prune :
    docker stop containerId   stop process
    docker kill containerId  

### Docker list

    docker ps 
    
    docker ps --all 

### Docker logs

    docker logs containerId


### Executing Commands in Running Containers

    docker exec -it containerId  redis-cli
    docker exec -it containerId  sh 

  Starting with Shell 

`docker run -it containerName sh`
**Note:**  -it : inside terminal

### Stop all 
    docker kill $(docker ps -q) 
### Remove All    

    docker rm $(docker ps -a -q) 
### Remove all images

    docker rmi $(docker images -a -q)
   **Note:**  
 

>   /var/run/docker.sock: connect: permission denied ?

       sudo setfacl -m "g:docker:rw" /var/run/docker.sock
       sudo addgroup --system docker 
       newgrp docker


------------------------------------------
## DOCKER BUILD

 1. FROM (Spacify a base image)
 2. RUN (Run some commands to install additional programs)
 3. CMD (Specify a command to run on container startup)
Example :

>         FROM alpine
>         //Download and install a dependency
>         `RUN apk add --update redis`    
>         //Tell the image
>         CMD ["redis-server"]

 `docker build . `
 
### Docker Build Add Tag
dockerID/ProjectName:version

    docker build -t deneme/redis:latest .

**Note:**  tags -  image : Version- Specifies the directory of files

    docker commit -c 'CMD ["redis-server"]' containerId

### Copying Build  Files

COPY   ./    ./   
| ./| ./  |
|--|--|
| Path to folder to copy from realative to build context |  stuff to inside the container |

Specifying a working directory

    WORKDIR /usr/app


### Port Mapping

    docker run -p localhostPort:port insideContainer imagename

### Docker File

```yaml
# Specify a base image
FROM  node:14-alpine
WORKDIR  /usr/app
# Install some depenendencies
COPY  ./package.json  ./
RUN  npm  install
COPY  ./  ./
# Default command
CMD  ["npm",  "start"]
```
-----------------------------------------------------------

## Docker compose
docker compose YML: 
```yaml
version: '3'
 services:
  redis-server:
   image: 'redis'
  node-app:
   build: .
   ports:
      - '4001:8081'
```
##### Launch in background            

    sudo docker-compose up -d

###### Stop Containers
    sudo docker-compose down


### Docker Restart Policies
- **no :** Never
- **always :** if this containers stops for any reason always attempt to restart it
- **on-failure:** Only restart if the container stops with an error code
- **unless-stopped:** Always restart unless we forcibly stop it
```yaml
version: '3'
 services:
  redis-server:
   image: 'redis'
  node-app:
   restart : always  *****
   build: .
   ports:
      - '4001:8081'
```

#### FILE  Operation

> WORKDIR /usr/app 
> COPY ./ /usr/app
> 
>     FROM  node:alpine
>     RUN  mkdir  -p  /home/node/app  &&  chown  -R  node:node  /home/node/app
>     WORKDIR  /home/node/app
>     RUN  chgrp  -R  0  /home/node/app  &&  chmod  -R  g+rwX  /home/node/app
>     COPY  package*.json  /home/node/app/
>     USER  1000
>     RUN  npm  install
>     COPY  --chown=node:node  .  /home/node/app
>     EXPOSE  3000
>     CMD  ["npm","run","start"]




## Docker Volume
    
    docker run -p 3000:3000 -v /node_modules -v $(pwd):/app containerID

Docker Compose:
```yaml
version: '3'
 services:
  web:
   build:
    context: .
    dockerfile: Dockerfile.dev
   ports:
     - '3000:3000'
   volumes:
     - /node_modules
     - .:/app

```



docker run -d -it --name devtest -v volume_name:/var/www nginx:latest
//Docker File 
FROM alpine
VOLUME ["/data"]
ENTRYPOINT ["/bin/sh"] 
///Docker Compose
version: '2'
services:
  webserver:
    build: .
    ports:
     - "9000:80"
    volumes:
     - .:/usr/share/nginx/html
Volumes ile çalışırken Container silerken Volume’ları da mutlaka yönetin. Yoksa sistemde gereksiz yere sismelere neden olabilir
----------------------------------------------
sudo docker run -p 3000:30000 -v /app/node_modules -v $(pwd):/app 3547e79546d9

---------------------------------------------------------
sudo gpasswd -a omerfarukicen docker
newgrp docker
sudo su omerfarukicen
-----------------------------------------------------------
