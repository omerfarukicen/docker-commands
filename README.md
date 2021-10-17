# docker-commands

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
    docker run -it containerName sh
**Note:**  -it : inside terminal
### Stop all 
    docker kill $(docker ps -q) 
### Remove All    

    docker rm $(docker ps -a -q) 
### Remove all images

    docker rmi $(docker images -a -q)
     


------------------------------------------
## DOCKER BUILD

 1. FROM 
 2. RUN 
 3. CMD
Example :

>         FROM alpine
>         //Download and install a dependency
>         `RUN apk add --update redis`    
>         //Tell the image
>         CMD ["redis-server"]

 `docker build . `
 
### Docker Build Add Tag

    docker build -t deneme/redis:latest .
    
**Note:**  tags -  image : Version- Specifies the directory of files

    docker commit -c 'CMD ["redis-server"]' containerId

### Docker Restart Policies
***no :*** Never
***always : *** if this containers stops for any reason always attempt to restart it
**on-failure:** Only restart if the container stops with an error code
**unless-stopped:** Always restart unless we forcibly stop it


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

FROM node:alpine

WORKDIR '/app'

COPY package.json .
RUN npm install

COPY . .

CMD ["npm", "run", "start"]

### Port Mapping
> `sudo docker run -p 3000:30000`
-----------------------------------------------------------

## Docker compose
docker compose YML: 

> version: '3' services:
>      redis-server:
>         image: 'redis'
>      node-app:   
>         build: .
>         ports:
>             -"4001:8081"

##### Launch in background            

    sudo docker-compose up -d

###### Stop Containers

    sudo docker-compose down

## Docker Volume
Docker Volumes, Docker Container’larındaki verileri saklamamız veya Container’lar arasında veri paylaşmamız gerektiğinde çok kullanışlıdır. Docker Volumes çok önemli bir kavramdır. Çünkü Docker Container silindiğinde tüm dosya sistemi de yok edilir. Bu gibi durumlarda verileri bir şekilde saklamak istiyorsak, Docker Volumes kullanmamız gerekiyor.

    docker volume create volume_name  //create
    docker volume rm volume_name     // REMOVE
    
    docker build -f Docker .
    docker run -p 3000:3000 -v /app/node_modules -v $(pwd):/app b8fe0b019bf4


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
