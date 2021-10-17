# docker-commands

docker create hello-world



--------------START --------------------
docker start -a "dockerID"
docker run    "imageName"
docker start  "containerId"

Docker Run is going to show you all the logs or al the information
Docker start opposite but quickly
------------------------------------------------------
  Stop 
----------------------------------------
docker system prune :
docker stop containerId   stop process
docker kill containerId  
--------------------List working-----------------

docker ps 

docker ps --all 
-----------------------------------------------
------------------------LOGS-----------
docker logs containerId
-------------------------------------------
Executing Commands in Running Containers
--------------------------------------
docker exec -it containerId
docker exec -it containerId  redis-cli
-it : inside terminal
docker exec -it containerId  sh 
docker run -it containerName sh
------------------------------------------
DOCKER BUILD
-----------------------------

FROM
RUN
CMD
Example :


# Use an existing docker image
FROM alpine

#Download and install a dependency

RUN apk add --update redis

#Tell the image

CMD ["redis-server"]

docker build . 
------------------------------------
Docker Build Add Tag
-----------------------------
Docker build -t deneme/redis:latest .

tags -  image : Version- Specifies the directory of files

-------------------------------------------------
MANUEL DOCKER BUILD
docker run -it alpine sh
apk add --update redis

docker commit -c 'CMD ["redis-server"]' containerId
--
FILE İşlemleri
WORKDIR /usr/app
COPY ./ /usr/app

Port Mapping


------------------------------------------
Docker compose
docker compose YML
version: '3'
services:
     redis-server:
        image: 'redis'
     node-app:   
        build: .
        ports:
            -"4001:8081" 
Launch in background            
sudo docker-compose up -d

Stop Containers
sudo docker-compose down
docker kill $(docker ps -q)  Stop ALL Docker
docker rm $(docker ps -a -q) Remove All Docker Container
---------------------------------------------------------
Docker Restart Policies
no : Never
always : if this containers stops for any reason always attempt to restart it
on-failure: Only restart if the container stops with an error code
unless-stopped: Always restart unless we forcibly stop it

npm run start 
npm run build 

---------------------------
Project Dockerize
FROM  node:alpine
WORKDIR  '/app'

COPY  package.json .

RUN npm install

COPY . .
 
CMD ["npm","run","start"]

sudo docker build -f Dockerfile.dev .
docker run -it -p 3000:3000 IMAGE_ID
----------------------------------
Docker Volume
Docker Volumes, Docker Container’larındaki verileri saklamamız veya Container’lar arasında veri paylaşmamız gerektiğinde çok kullanışlıdır. Docker Volumes çok önemli bir kavramdır. Çünkü Docker Container silindiğinde tüm dosya sistemi de yok edilir. Bu gibi durumlarda verileri bir şekilde saklamak istiyorsak, Docker Volumes kullanmamız gerekiyor.

docker volume create volume_name  //create
docker volume rm volume_name     // REMOVE
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
docker rmi $(docker images -a -q)  // REMOVE ALL IMAGES
---------------------------------------------------------
sudo gpasswd -a omerfarukicen docker
newgrp docker
sudo su omerfarukicen
-----------------------------------------------------------





