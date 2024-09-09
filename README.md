# Docker Workshop 

This repo contains a list of commands used on workshop "Docker". It includes also sample application we can build and run by own or using https://killercoda.com/docker


## Exercise 1

Running first containers:

```sh
#test run
docker container run hello-world
docker run buysybox echo Hello Tester Day

#Playing around with figlet
docker container run -it ubuntu bash
figlet hello
apt update
apt install figlet     
figlet Hello Tester Day

docker container run -it -d ubuntu bash
docker container ls
docker container exec -it <container_id> bash
```


### NGINX

Port exposing with nginx on 80
```sh
docker pull nginx
docker container run -d -p 80:80 --name mynginx nginx
```


## Exercies 2 

How to view output

```sh
#simple output
docker run  jpetazzo/clock          
docker run -d jpetazzo/clock        
docker logs {ID_CONTAINER}          
docker logs {ID_CONTAINER} -f | less
docker ps                           
docker stop                         

##unix kill signal 10s timeout      
docker kill                         
```

### Layers and Tagging
Showing Layers and how to preserve state

```sh 
docker run -it ubuntu:12.04           
cat /etc/os-release                   

##creating layer                               
docker run -it ubuntu                 
figlet hello                          
apt update                            
apt install figlet                    

#saving layer
docker diff {ID_CONTAINER}            
docker commit {ID_CONTAINER}          
docker images                         
docker run -it {ID_IMAGE}             
history                               
figlet                                
docker images                         
docker tag --help                     
docker tag {ID_IMAGE} {NAME_IMAGE}    
docker run my_figlet  figlet message  
```

### Dockerfile demo


```sh
### Dockerfile                                                                     
cat <<EOL > dockerfile.yaml                                                        
FROM ubuntu                                                                     
RUN apt update                                                                  
RUN apt install figlet                                                          
EOL                                                                             

docker build dockerfile.yaml                                                                 
docker images                                                                   
docker run {ID_IMAGE}  figlet dockerfile.yaml                                        
```
                                                                                
### Build cache and taggind                                                       
```sh
docker build -t {TAG_NAME} dockerfile.yaml
docker images                                                                   
docker history {IMAGE_NAME}                                                     
```

### Container sizes and alpine                                                    

```sh
docker pull nginx                                                               
docker pul  nginx:alpine                                                        
                                                                                
```

### Multibuild stage

Demonstrating compiled application and using multisatge build to optimize container

```sh
docker build -t hello 2dockerfile.yaml
```

## Exercise 3

Modifying Nginx files:

```sh
#testing nginx and modifing file
docker  run --rm --name my_rick_roll -p 8080:8080 modem7/docker-rickroll:latest 
docker  run --rm --name my_nginx -p 8080:80 nginx                               
docker exec -it my_nginx  bash                                                  

#demo files
cat <<EOL > /usr/share/nginx/html/index.htnl                                      
<!DOCTYPE html>                                                                   
<html>                                                                            
<head>                                                                            
<title>Welcome to Tester Days!</title>                                            
<style>                                                                           
html { color-scheme: light dark; }                                                
body { width: 35em; margin: 0 auto;                                               
font-family: Tahoma, Verdana, Arial, sans-serif; }                                
</style>                                                                          
</head>                                                                           
<body>                                                                            
<h1>Welcome to Tester Days!</h1>                                                  
<p>If you see this page, the nginx web server is successfully installed and       
working. Further configuration is required.</p>                                   
                                                                                  
<p>For online documentation and support please refer to                           
<a href="http://nginx.org/">nginx.org</a>.<br/>                                   
Commercial support is available at                                                
<a href="http://nginx.com/">nginx.com</a>.</p>                                    
                                                                                  
<p><em>Thank you for using nginx.</em></p>                                        
</body>                                                                           
</html>                                                                           
```

## Exercise 4

Orchestrating and scailing  multiple coing mainter containers with docker compose 

```sh
cd dockercoins 
docker-compose up -d
docker-compose scale worker=3
docker-compose top 
docker-compose ps  
docker-compose down
```


## Exercise 5

Running WordPress using docker-compose:

```sh
cd wordpress
docker-compose up -d
docker-compose down
```

To delete volumes data use following command:
```sh
docker-compose down -v
