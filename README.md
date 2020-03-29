## Install Maven

1) Start by updating the package index  
       
       sudo apt update
2) Next, install Maven by typing the following command
    
       sudo apt install maven
3) Verify the installation

       mvn -version
The output should look something like this:

      Apache Maven 3.6.0
      Maven home: /usr/share/maven
      Java version: 11.0.6, vendor: Ubuntu, runtime: /usr/lib/jvm/java-11-openjdk-amd64
      Default locale: en, platform encoding: UTF-8
      OS name: "linux", version: "4.15.0-1057-aws", arch: "amd64", family: "unix"
      root@ip-172-31-91-171:/home/ubuntu/qwerty/kubernetes# 

## Build with Maven
```shell
mvn package
```
## Create Dockerfile

### Create file
       vi Dockerfile
### Write dockerfile commands script
       FROM openjdk:8-jdk-alpine
       VOLUME /tmp
       ADD target/gs-spring-boot-docker-0.1.0.jar app.jar
       ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]
       
FROM directive defines the base image to use to start the build process.

VOLUME command is used to enable access from your container to a directory on the host machine

ADD command gets two arguments: a source and a destination. It basically copies the files from the source on the host            into the container’s own filesystem at the set destinationv       

ENTRYPOINT argument sets the concrete default application that is used every time a container is created using the image.
## Containerize
1. Build a docker image using `Dockerfile`:
   
       docker build -t web-app .
   
2. Run docker image locally
   
       docker run --rm -p 8080:8080 web-app
   
3. Then you can access the web app at http://localhost:8080 in browser.

Or you can pull this sample from my Docker Hub. https://hub.docker.com/repository/docker/tigranabovyan/sample-java-web-app


## Deploy to K8S

1) Create Deployment.(You can change resources limits and requests)
```
       kubectl create -f deploy.yaml
```
2) Create load balancer service.
```
       kubectl create -f service.yaml
```
3) Create Horizontal Pod Autoscaler (you can set your own utilization percentages )
```
       kubectl create -f hpa.yaml
```
Then you can access the web app at http://EXTERNAL-IP-OF-LOAD-BALANCER:8080 in browser

To get EXTERNAL-IP of your load balancer service run following command
       
       kubectl get svc web-app-svc

To monitor your deployment average cpu and memory usage, replicas run following command

       kubectl get hpa web-app-hpa 
       
