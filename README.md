# Micro-Services
Synechron Training for Java Microservices.

Day1:
For resources visit: 
* [LAB RESOURCES](http://resources.way2learnonline.com/micro/) (Commited lab-docs-pdf-jan2018.zip and micro-ws-may2018.zip)

## Monolithic Applications VS Micro-services
### Monolithic Applications: One single appn having everything is called as monolithic appn
Disadvantages:
  1. One single appn having everything is called as monolithic appn, hence this is the biggest disadv.
  2. Developers take time to understand the appn
  3. Tightly coupled with technology stack, a change in tech can cause serious issues.
  
### Micro-services: One big application with small small services
**Advantages:**
  1. Dev can become productive quickly
  2. Scaling the services is easy
  3. Every service can be developed in language of choice : eg- Search service in scala, cart service in ruby, etc; its easy to leverage        and use new technolgies due to this.
  4. Utilization of hardware resources is good.
  
**Disadvantage:**
  1. All the problems of distributed systems are present as microservice communicate using distributed calls.
  
Netflix Eureka library: For discovery service
Netflix Ribbon library: For client side load-balancing

  Zones: 
  - having all the service instances and individual eureka instances
  - Every Eureka instances should be having the rtegistry of their own services and sync them up with other eureka instances in different zones. (Instances of eureka should be static may 2 or 3 instances)
      Refer example.png in the workspace folder

### Circuit issue in micro-services:
````
Example:
If service 
  s1 is dependent on s2
  s2 is dependent on s3, s4
  s3 is dependent on s4
````
This is above where services create a circuit in within, so if S4 is down then we have a downfall in network then we should have a FALLBACK mechanism. However one failure should never be treated as circuit, there should be failure and success counters and if the % of failure count is more than specific then only it should be treated as circuit.
For this issue Netflix already has a library namely ***NETFLIX HYSTRIX***
>>>This library has to be present in the all the services as we have to check for circuit before making any request to other service.
````
Example :
S1 wants to make a call to S2:
S1 ---> Eureka (DS) 
  Eureka (DS) ---> S1
  S1 ---> Ribbon (LB)
  S1 ---> HYSTRIX --> S2
````

**Exposing internal services to external services**
No external service should be able to access my service architecture hence we should be having a proxy interface which will call the service on behalf of us. To acheive this there is a third party service provided by netflix **ZUUL**
````
Example:
ES (external service) wants to make a call to (your internal service) S2:
ES --> Netflix Zuul ---> Eureka (DS) 
  Eureka (DS) ---> Zuul
  Zuul ---> Ribbon (LB)
  Zuul ---> HYSTRIX --> S2
So here Zuul will be the interface which will handle all the responsibilities i.e. LB, DS and calling the target service.
````
**Having configuration of all the services on single server**
  
## SPRING BOOT for implementing microservices
  1. Automatic configuration
  2. If you just add module in the classpath, then all the beans required for added module will be created by boot.
    Eg: if you add JPA in classpath then all beans related to JPA will be created.
  3. No unnecessary JAR should be added in classpath or else unecessary beans will be created.
  4. In order to use boot then in pom.xml you need to add below code: (this code is must)
          <parent>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-parent</artifactId>
            <version>2.0.0.RC1</version>
            <relativePath />
          </parent>
  5. you can then go ahead and provide the required artifacts in the dependencies.
  6. once you add the required dependencies you have to use ***@AutoConfiguration*** annotation in the class
  
  ### How spring boot works:
   1. Read bean post processor (life cycle of spring bean)
   
# DAY 2 : REST/EUREKA/RIBBON
## CLOUD
  1. What is CLoud?
    - **SaaS**
      . SAP
      . Source Fourge
    - **PaaS**
      . Pivotal Cloud foundry
      . Heroku
    - **IaaS**
      . AWS
      . Google Cloud Platform
      . Azure
  - Whatever is not yours is with cloud. 
  - If we use IaaS : we just get the plain OS infrastructure and if I want to run a WAR on it we have to configure all the pre-requisite
  - If we use PaaS : we just have to say we want to run WAR then all the pre-requisite are handled by Platform
  - In SaaS : all the code is ready we just have to configure.
  
  **Problem statement:**  I want to write an appn which is cloud independent: i.e I can switch within clouds without changing code?
  **Solution:** ***Spring cloud*** provides set of interfaces which have implementors of Heruko, Cloud platform etc. Just change the dependency in spring boot and the required beans will be created.
  
  ## SPRING CLOUD
  - Its a library which can be deployed on PaaS
  - Its a big umbrella which has different projects like Eureka, ribbons, etc.
  
  ## How to start Eureka:
  - Eureka is just an REST api.
  - Spring cloud eureka is developed on top of spring boot, hence boot will add all the beans for you once you add the dependency
  - Spring boot parent doesnt have dependecy versions for spirng cloud api's hence we need to have different parent for spring cloud but we cannot have multiple parents in one pom so we add the cloud parent in <dependencyManagement> like below: 
* [Spring cloud](http://projects.spring.io/spring-cloud/)

````
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.0.1.RELEASE</version>
</parent>
<dependencyManagement>
    ***<dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-dependencies</artifactId>
            <version>Finchley.SR1</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>***
</dependencyManagement>
<dependencies>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-config</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
    </dependency>
</dependencies>
````
Please make sure you have correct compatibilty version of sprint boot parent and spring cloud.

    
  
    
       
