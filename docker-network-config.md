

# Setup
> sudo apt install docker.io
> sudo apt install openjdk-11-jdk
> sudo apt install mvn

## Java code Chanage in producer for rest call:

```java
@GetMapping("employee")
public String getEmployee() { 
  RestTemplate restTemplate = new RestTemplate(); 
  ResponseEntity<?> response=restTemplate.getForObject("http://consumer:8087/employee",,String.class); 
  return response.getBody(); 
}
``` 
## Create Springboot microservice jars

> mvn clean install

## Dockefile
```
From openjdk:8
copy ./target/employee-producer-0.0.1-SNAPSHOT.jar employee-producer-0.0.1-SNAPSHOT.jar
CMD ["java","-jar","employee-producer-0.0.1-SNAPSHOT.jar"]
```
file2
```
From openjdk:8
copy ./target/employee-consumer-0.0.1-SNAPSHOT.jar employee-consumer-0.0.1-SNAPSHOT.jar
CMD ["java","-jar","employee-consumer-0.0.1-SNAPSHOT.jar"]
```
## Build images from projects
> docker image build -f Dockerfile -t employee-producer .
> docker image build -f Dockerfile -t employee-consumer .

## Create Docker Network
> docker network ls
> docker network create microservices-network

# Deploy images in container
> docker run --name producer --network microservices-network -p 8083:8083 employee-producer
> docker run --name consumer --network microservices-network -p 8087:8087 employee-consumer



