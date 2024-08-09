# Microservices

WebService: It should be platform independent(interoprable), machine to machine(or application to application), should allow communication over a network.
Service definition: Request/Response format, Request structure, Response structure, Endpoint.
How we make it platform independent: by using the data exchange formats like XML, JSON.
Need to define the transport. If we are using over the internet it has to be: HTTP, communication over Queue: MQ(Messaging queue) (Basically Servive provider would be listening to the WebSphere MQ, where the service requestor would be putting its request. After processing the request, provider will put the response in the same MQ which the requestor is listening)

WebService groups: SOAP-based, REST-styled. Both are not comparable. 
SOAP puts restrictions on the format. Simple object access protocol (SOAP). We use XML as request exchange format. Soap envelope has a header and a body. It can communicate on MQ or over the internet HTTP. Service definition is typically done using WSDL(Web service definition language).
REST (Representational state transfer) HTTP Methods (GET, POST, PUT, DELETE), HTTP Status code (200, 404...). Resource: anything that you want to expose to outside world. A resource has an URI(Uniform resource identifier): /user/todo/1, /user/todo/anurag. A resource can have different representations: XML, HTML, JSON. Rest doesn't put any restriction on exchange format. But it works on only HTTP. No standard service definition: WADL (Web application definition language), Swagger

============
Spring Boot
============
Before spring boot, how spring projects were setup:
1) Dependency management (pom.xml)
2) Web app configuration (web.xml)
3) Manage spring beans (context.xml)
4) Implement non functional requirements (NFRs)
Lot of days to setup and run

Goal of springboot: build production ready apps quickly. Auto configuration, Dependency management is all done by spring boot.
Ex. If we add the dependency Spring Boot Starter Web, following are added and configured automatically
- Disapatcher Servlet(DispatcherServletAutoCOnfiguration)
- Embedded Servlet Container: Tomcat is default (EmbeddedWebServerFactoryCustomizerConfiguration)
- Default error page (ErrorMvcAutoConfiguration)
- Bean to Json (JacksonHttpMessageConvertersConfiguration)

There are 4 things using which you can build the web applications fast:
- Spring initializer
- Spring starter project
- Spring Auto configuration
- Spring dev tools (to increase productivity ex. no need to restart the server for every change, Manage different configuration for different environment)

==========
Method reference (Functional programming)
System.out::println   =>   	Classname::static method name to call that method

==========
How to deploy your application?

OLD approach
- Install Java
- Install Web/Application server (Tomcat/WebSphere/WebLogic etc)
- Deploy the application WAR(Web archive)

EMbedded approach
- Install Java
- Run JAR file where Tomcat is already embedded into the JAR file

======
1) First the request goes to DispatcherServlet. From DispatcherServlet requests are redirected to corresponding controllers. How DispatcherServlet is configured? In Spring boot, DispatcherServlet it auto configured.
2) If the response returned is an object it is automatically converted to json. This is done using JacksonHttpMessageConverters which is also auto configured by Spring boot in AutoConfiguration. 
3) Who is mapping error page: This is also auto configured by Spring boot using ErrorMvcAutoConfiguration when an unknown api is hit for which mapping is not done.
4) How all the Jars are available(Spring, Spring MVC, Jackson, Tomcat)? Using starter projects: Spring boot starter web (spring-webmvc, spring-web, spring-boot-starter-tomcat, spring-boot-starter-json)

======
Spring boot actuator
Monitor and manage your application in your application. Provide number of endpoints
- beans: complete list of spring beans in your app
- health: application health information
- metrics: application metrics
- mapping: details around request mappings 

======
Spring framework: Dependency injection: @Component @Autowired @ComponentScan. Spring provides good integration with other frameworks(Hibernate/JPA, Junit, Mockito)

Spring MVC: simplifying building web apps and REST api. Earlier struts was used but it was very complex. You can use @Controller, @RestController @RequestMapping("/courses")

Spring Boot: Build production ready apps quickly. They have starter projects and auto configuration. Eliminate configuration to setup Spring, Spring MVC and other frameworks. Enables NFRs: Actuators, Embedded server, Loggin and erroe handling, profile and configuration properties.

======
Important status codes:
200 -> Success
201 -> Created
204 -> No content
401 -> Unauthorized
400 -> Bad request
404 -> Resource not found
500 -> Server error

======
REST API Documentation: Swagger and Open API
2011: Swagger specification and Swagger tools were introduced.
2016: Open API specification created based on Swagger Spec.: Swagger Tools (Swagger UI) continue to exist.
Open API specification: Standard, language agnostic interface. Discover and understand REST API. Earlier called Swagger specification.
Swagger UI: Visualize and interact with your REST API

springdoc-openapi java library helps to automate the generation of API documentation using spring boot projects. springdoc-openapi works by examining an application at runtime to infer API semantics based on spring configurations, class structure and various annotations.
Automatically generates documentation in JSON/YAML and HTML format APIs. This documentation can be completed by comments using swagger-api annotations.

======
Content negotiation: Consumer can tell the Rest API provider in which format they want the resource. They want either the JSON or XML or in what other language. 
You can create Accept header (MIME type: application/xml, application/json). For language: you can put accept-language header (en, nl, fr...)
Need to add the dependency as com.fasterxml.jackson.dataformat and then add in headers application/xml for Accept parameter.

=====
Internationalization- i18n
You need to send the values as per the desire in header: Accept-Language, indicates natural language and locale that the consumer prefers. en, fr, nl

=====
Versioning can be done in variety of ways:
- URL
- Request Parameter
- Header
- Media type (MEME type)

Versioning using this create URL pollution: URL and Request parameter
Load on HTTP as Header and Media type is never meant for versioning.
Caching only happens in URLs. So when talking about Header versioning and media type versioning caching also needs to look into headers and media types.
URL and Request parameter versioning request can be executed on the browser.
API documentation can easily be made for URL and request parameter versioning. 

======
HATEOAS: Hypermedia as the engine of application state. Websites allow you to see data and perform actions(using links). 
HAL: Json Hypertext application language. Simple format that gives a consistent and easy way to hyperlink between resources in your API. Spring Hateos generates HAL responses with hyperlinks to resources.

HAL Explorer: An api explorer for Restful hypermedia apis using HAL. Enable your non-technical teams to play with APIs.

Spring Boot HAL Explorer: auto configures HAL explorer for spring boot projects 

======
Serialization: Convert json to string/ string to json. Most popular JSON serialization in JAVA: Jackson. Customizing the REST API response returned by Jackson framework.
1) Cusomized field names in response @JsonProperty
2) Return only selected fields: Filtering
Static filtering: Same filtering for a bean across different REST API using @JsonIgnoreProperties @JsonIgnore
Dynamic filtering: Cutomized filtering for a bean for specific REST API. @JsonFilter with FilterProvider. 

======
Spring boot actuator: provides spring boot's production ready features. Monitor and manages your application in your production. 
Provides a number of endpoints:
1) beans: complete list of spring beans in your app
2) health: application health information
3) metrics: application metrics
4) mappings: details around request mappings

======
JDBC: JDBC is a programming-level interface for Java applications that communicate with a database. An application uses this API to communicate with a JDBC manager. It's the common API that our application code uses to communicate with the database. Beyond the API is the vendor-supplied, JDBC-compliant driver for the database we're using.

JPA: JPA is a Java standard that allows us to bind Java objects to records in a relational database. It's one possible approach to Object Relationship Mapping(ORM), allowing the developer to retrieve, store, update, and delete data in a relational database using Java objects. Several implementations are available for the JPA specification.

======
JPA vs JDBC
When it comes to deciding how to communicate with back-end database systems, software architects face a significant technological challenge. The debate between JPA and JDBC is often the deciding factor, as the two database technologies take very different approaches to work with persistent data.
JDBC allows us to write SQL commands to read data from and update data to a relational database. JPA, unlike JDBC, allows developers to construct database-driven Java programs utilizing object-oriented semantics. The JPA annotations describe how a given Java class and its variables map to a given table and its columns in a database. The JPA framework then handles all the time-consuming, error-prone coding required to convert between object-oriented Java code and the back-end database. 
When associating database tables in a query with JDBC, we need to write out the full SQL query, while with JPA, we simply use annotations to create one-to-one, one-to-many, many-to-one, and many-to-many associations. Let's say our employee table has a one-to-many relationship with the communication table. The owner of this relationship is Communication, so we're using the mappedBy attribute in Employee to make it a bi-directional relationship.

JDBC is database-dependent, which means that different scripts must be written for different databases. On the other side, JPA is database-agnostic, meaning that the same code can be used in a variety of databases with few (or no) modifications.

Because JDBC throws checked exceptions, such as SQLException, we must write it in a try-catch block. On the other hand, the JPA framework uses only unchecked exceptions, like Hibernate. Hence, we don't need to catch or declare them at every place we're using them.

JPA-based applications still use JDBC under the hood. Therefore, when we utilize JPA, our code is actually using the JDBC APIs for all database interactions. In other words, JPA serves as a layer of abstraction that hides the low-level JDBC calls from the developer, making database programming considerably easier.

The most obvious benefit of JDBC over JPA is that it's simpler to understand. On the other side, if a developer doesn't grasp the internal workings of the JPA framework or database design, they will be unable to write good code.

Also, JPA is thought to be better suited for more sophisticated applications by many developers. But, JDBC is considered the preferable alternative if an application will use a simple database and we don't plan to migrate it to a different database vendor.

The main advantage of JPA over JDBC for developers is that they can code their Java applications using object-oriented principles and best practices without having to worry about database semantics. As a result, development can be completed more quickly, especially when software developers lack a solid understanding of SQL and relational databases.

======
Difference between JDBC and Spring JDBC
- in both we have to write lot of SQL(structured query language) queries. But in spring JDBC less java code is required.

JDBC:
public void deleteTodo(int id){
	PreparedStatement st = null;
	try {
		st = db.conn.prepareStatement("delete from todo where id=?");
		st.setInt(1,id);
		st.execute();
	} catch(SQLException e) {
		logger.fatal("Query Failed : ", e);
	} finally {
		if(st != null) {
			try {st.close();}
			catch (SQLException e) {}
		}
	}
}

Spring JDBC:
public void deleteTodo(int id){
	jdbcTemplate.update("delete from todo where id=?", id);
}

======
JPA over JDBC: You don't need to write the queries. The pojo itself is considered as the table and its members as the column.

Spring JPA: You don't need to create the functions itslef. Just and interface and everything is done.

======
JPA vs Hibernate
JPA defines the specification. Is is an API. It tells how to define entities, how do you map attributes, who manages the entities?. Hibernate is one of the popular implementations of JPA. Using hibernate directly result in a lock in to Hibernate. There are other JPA implementations (Toplink, for example)

================
MICROSERVICES
================
In short, the microservice architectural style is an approach to developing a single application as a suite of small services, each running in its own process and communicating with lightweight machanisms, often an HTTP resource API. These services are built around business capabilities and independently deployable by fully automated deployement machinary. There is a bare minimum of centralized management of these services which may be written in different programming languages and uses different data storage technologies.

So microservices are:
1) Rest services
2) Small well chosen deployable units
3) Cloud enabled

OR

1) Independently deployable
2) Loosely coupled, tesable and deployable architecture.
3) Organized around business capabilities
4) Owned by a small team

Challenges in microservices:
1) You need to decide the boundary of microservices.
2) Configuration management (there can be 10 services in 5 environments having 15 instances).
3) Need to have dynamic scale up and scale down: Load balancing
4) To find the bug if feature is distributed across 4-5 microservices, having monitoring and centralized log. Which api is down and where?
5) Pack of cards: if not designed properly 

======
Spring cloud: There is not one project under spring cloud. There are wide variety of projects under spring cloud. One of the major importart project under spring cloud is 
- Spring cloud netflix: Integration with various Netflix OSS components(Eureka, Hystrix, Zuul, Archaius, etc.). 
- Spring cloud config: Spring cloud config server provides an approach where you can store all the configuration for all the different environments for all the microservices just at one place in centralized location. You can store configuration for all the environments microservices just at one place. And spring cloud config server used to expose that configuration to all microservices. Spring Cloud Config provides server-side and client-side support for externalized configuration in a distributed system. With the Config Server, you have a central place to manage external properties for applications across all environments. 

To solve the challenge for having the configurations for number of microservices for all environments for different instances, we can use Spring cloud config, having centralized location where configurations can be saved

======
Scale up and scale down
Naming server (Eureka): all the instances of all microservices would register with the naming server. Naming services has 2 important features: 1) Service registration 2) Service discovery. Eureka Server is an application that holds the information about all client-service applications. Every Micro service will register into the Eureka server and Eureka server knows all the client applications running on each port and IP address. Eureka Server is also known as Discovery Server. Eureka Server comes with the bundle of Spring Cloud. Eureka naming server is a REST-based server that is used in the AWS Cloud services for load balancing and failover of middle-tier services. Eureka naming server is an application that holds information about all client service applications. Each microservice registers itself with the Eureka naming server. The naming server registers the client services with their port numbers and IP addresses. It is also known as Discovery Server.  Eureka naming server comes with the bundle of Spring Cloud. It runs on the default port 8761. It also comes with a Java-based client component, the eureka client, which makes interactions with the service much easier.

Ribon for client side load balancing. Netflix Ribbon is a Part of Netflix Open Source Software (Netflix OSS). It is a cloud library that provides the client-side load balancing. It automatically interacts with Netflix Service Discovery (Eureka) because it is a member of the Netflix family. The Ribbon mainly provides client-side load balancing algorithms. It is a client-side load balancer that provides control over the behavior of HTTP and TCP client. The important point is that when we use Feign(to write easier rest services), the Ribbon also applies.
Features of Ribbon
1) Load balancing
2) Fault tolerance
3) Multiple protocol support in Asynchronous model
4) Caching and batching
We will use Feign for writing easier REST client.

======
Visibility and monitoring
1) Zipin distributed tracing: Zipkin is a distributed tracing system. It helps gather timing data needed to troubleshoot latency problems in service architectures. Features include both the collection and lookup of this data. If you have a trace ID in a log file, you can jump directly to it. Otherwise, you can query based on attributes such as service, operation name, tags and duration. Some interesting data will be summarized for you, such as the percentage of time spent in a service, and whether or not operations failed. Distributed tracing is a technique used to profile and monitor applications, especially those built using the microservice architecture. Distributed tracing, also called distributed request tracing. IT and DevOps teams can use distributed tracing to monitor applications. It identifies the failed microservices or the services having performance issues when there are many services call within a request. It is very useful when we need to track the request passing through the multiple microservices. It is also used for measuring the performance of the microservices. It assigns an ID to each request. So, it is easy to trace each request using ID.

2) Netflix API Gateway: In a microservices architecture, we typically have many system components independently deployed from each other. This brings problems like:
1) UI developers need to know the address of each microservice they need to consume
2) CORS - Cross Origin Resource Sharing
3) Authentication & Authorization implementation for each system component

3) A zuul proxy is used as an API gateway which receives all the incoming requests to the system and calls corresponding underlying microservice based on the configurations. With this architecture:
1) We won’t have CORS issues
2) We will have a centralized security implementation which will be applied to all incoming requests
3) We can apply filters to all incoming requests
4) When needed, we will be able to change the security business logic easily (since it is centralized)
Notice that the zuul proxy is another microservice meaning that it is also scalable.

4) Fault tolerance: Hyterix: Fault tolerance refers to the ability of a system (computer, network, cloud cluster, etc.) to continue operating without interruption when one or more of its components fail. The objective of creating a fault-tolerant system is to prevent disruptions arising from a single point of failure, ensuring the high availability and business continuity of mission-critical applications or systems. Consider a scenario in which six microservices are communicating with each other. The microservice-5 becomes down at some point, and all the other microservices are directly or indirectly depend on it, so all other services also go down. The solution to this problem is to use a fallback in case of failure of a microservice. This aspect of a microservice is called fault tolerance. Fault tolerance can be achieved with the help of a circuit breaker. It is a pattern that wraps requests to external services and detects when they fail. If a failure is detected, the circuit breaker opens. All the subsequent requests immediately return an error instead of making requests to the unhealthy service. It monitors and detects the service which is down and misbehaves with other services. It rejects calls until it becomes healthy again. Hystrix is a library that controls the interaction between microservices to provide latency and fault tolerance. Additionally, it makes sense to modify the UI to let the user know that something might not have worked as expected or would take more time.

======
Advantages of Microservices:
1) Each microservice can be written in different languages. Not like monolithic services where every service is written in same language. 2) Microservices are dynamic scaling 
3) Faster release cycles. Bringing new features faster to market.

======
Spring cloud gateway is simple yet effective way to route to APIs. Provide cross cutting concerns: Security, Monitoring/metrics. Built on top of Spring WebFlux(Reactive approach). Match request to any request attribute. Define predicates and filters. Integrates with Spring cloud discovery client(Load balancing). Path rewriting.

======
Circuit Breaker to reduce the load by not hitting the service which is down just returning the default fallback response. Can we write the fallback response is service is down? Can we implement Circuit breaker pattern to reduce load? Can we retry requests in case of temporary failures? Can we limit rate limiting?

Resilinece4j: is a lightweight, easy to use fault library inspired by Netflix Hystrix but designed for Java 8 and functional programming. return fallback response if service is down. Implements circuit breaker pattern to reduce load. Retry requests in case of temporary failures. Implements rate limiting. Resilience4j is a lightweight fault tolerance library designed for functional programming. Resilience4j provides higher-order functions (decorators) to enhance any functional interface, lambda expression or method reference with a Circuit Breaker, Rate Limiter, Retry or Bulkhead. You can stack more than one decorator on any functional interface, lambda expression or method reference. The advantage is that you have the choice to select the decorators you need and nothing else.

======
In the latest changes in microservices we will be using
Load Balancer: Ribbon => Spring cloud loadbalancer
API Gateway: Zull => Spring cloud Gateway
Fault tolerance: Hystrix => Resilience4j

======
YAML (.yml) File: YAML is a configuration language. Languages like Python, Ruby, Java heavily use it for configuring the various properties while developing the applications. It also supports list.
#.properties file
# A List
numbers[0] = one
numbers[1] = two
# Inline List
numbers = one, two

.properties File: This file extension is used for the configuration application. These are used as the Property Resource Bundles files in technologies like Java, etc. .properties file doesn’t support list but spring uses an array as a convention to define the list in the .properties file. 

Strictly speaking, .yml file is advantageous over .properties file as it has type safety, hierarchy and supports list but if you are using spring, spring has a number of conventions as well as type conversions that allow you to get effectively all of these same features that YAML provides for you. 

One advantage that you may see out of using the YAML(.yml) file is if you are using more than one application that read the same configuration file. you may see better support in other languages for YAML(.yml) as opposed to .properties.

========
Spring security: There are number of steps taken care by spring security when a webservice is hit. There are set of checks done called filter chains. Number of filter chains are done.
1) All request should be authenticated.
2) If a request is not authenticated then webpage is shown
3) CSRF: it will affect POST, PUT

If we want to change even one step of the chain, the whole chain needs to be defined again. 

=======
DOCKER
=======
Docker is a software platform that allows you to build, test, and deploy applications quickly. Docker packages software into standardized units called containers that have everything the software needs to run including libraries, system tools, code, and runtime. Similar to how a virtual machine virtualizes (removes the need to directly manage) server hardware, containers virtualize the operating system of a server. Docker is installed on each server and provides simple commands you can use to build, start, or stop containers.

Docker is an open source platform that enables developers to build, deploy, run, update and manage containers—standardized, executable components that combine application source code with the operating system (OS) libraries and dependencies required to run that code in any environment.

Containers simplify development and delivery of distributed applications. They have become increasingly popular as organizations shift to cloud-native development and hybrid multicloud environments. It’s possible for developers to create containers without Docker, by working directly with capabilities built into Linux and other operating systems. But Docker makes containerization faster, easier and safer.

Containers are made possible by process isolation and virtualization capabilities built into the Linux kernel. These capabilities—such as control groups (Cgroups) for allocating resources among processes, and namespaces for restricting a processes access or visibility into other resources or areas of the system—enable multiple application components to share the resources of a single instance of the host operating system in much the same way that a hypervisor enables multiple virtual machines (VMs) to share the CPU, memory and other resources of a single hardware server. 

As a result, container technology offers all the functionality and benefits of VMs—including application isolation, cost-effective scalability, and disposability—plus important additional advantages:

Lighter weight: Unlike VMs, containers don’t carry the payload of an entire OS instance and hypervisor. They include only the OS processes and dependencies necessary to execute the code. Container sizes are measured in megabytes (vs. gigabytes for some VMs), make better use of hardware capacity, and have faster startup times. 

Improved developer productivity: Containerized applications can be written once and run anywhere. And compared to VMs, containers are faster and easier to deploy, provision and restart. This makes them ideal for use in continuous integration and continuous delivery (CI/CD) pipelines and a better fit for development teams adopting Agile and DevOps practices.

Greater resource efficiency: With containers, developers can run several times as many copies of an application on the same hardware as they can using VMs. This can reduce cloud spending.

========
VMs vs Docker

A virtual machine is a system which acts exactly like a computer. In simple terms, it makes it possible to run what appears to be on many separate computers on hardware, that is one computer. Each virtual machine requires its underlying operating system, and then the hardware is virtualized. They have full OS with its own memory management installed with associated overhead of virtual device drivers. In virtual machine, valuable resources are emulated for the guest OS and Hypervisor, which makes it possible to run many instances of one or more operating systems in parallel on a single machine. Every guest OS runs an indiviual entity from host system.

Docker is a tool that uses containers to make creation, deployment, and running of application a lot easier. It binds application and its dependencies inside a container. Share kernel and share application libraries.

Significant differences between two are their operating system support, security, portability, and performance.
OS: each virtual machine has its guest operating system above the host operating system, which makes virtual machines heavy. While on the other hand, Docker containers share the host operating system, and that is why they are lightweight. Sharing the host operating system between the containers make them very light and helps them to boot up in just a few seconds. Hence, the overhead to manage the container system is very low compared to that of virtual machines. The docker containers are suited for situations where you want to run multiple applications over a single operating system kernel. But if you have applications or servers that need to run on different operating system flavors, then virtual machines are required.

Security: The virtual machine does no share operating system, and there is strong isolation in the host kernel. Hence, they are more secure as compared to Containers. A container have a lot of security risks, and vulnerabilities as the containers have shared host kernel. Also, since docker resources are shared and not namespaced, an attacker can exploit all the containers in a cluster if he/she gets access to even one container. In a virtual machine, you don’t get direct access to the resources, and hypervisor is there to restrict the usage of resources in a VM.

Portability: Docker containers are easily portable because they do not have separate operating systems. A container can be ported to a different OS, and it can start immediately. On the other hand, virtual machines have separate OS, so porting a virtual machine is difficult as compared to containers, and it also takes a lot of time to port a virtual machine because of its size. For development purposes where the applications must be developed and tested in different platforms, Docker containers are the ideal choice.

Performance
Comparing Virtual machines and Docker Containers would not be fair because they both are used for different purposes. But the lightweight architecture of docker its less resource-intensive feature makes it a better choice than a virtual machine. As a result, of which containers can startup very fast compared to that of virtual machines, and the resource usage varies depending on the load or traffic in it. Unlike the case of virtual machines, there is no need to allocate resources permanently to containers. Scaling up and duplicating the containers is also an easy task compared to that of virtual machines, as there is no need to install an operating system in them.

========
Docker lets developers access these native containerization capabilities using simple commands, and automate them through a work-saving application programming interface (API). Compared to LXC, Docker offers:

Improved and seamless container portability: While LXC containers often reference machine-specific configurations, Docker containers run without modification across any desktop, data center and cloud environment. 
Even lighter weight and more granular updates: With LXC, multiple processes can be combined within a single container. This makes it possible to build an application that can continue running while one of its parts is taken down for an update or repair. 

Automated container creation: Docker can automatically build a container based on application source code. 

Container versioning: Docker can track versions of a container image, roll back to previous versions, and trace who built a version and how. It can even upload only the deltas between an existing version and a new one. 

Container reuse: Existing containers can be used as base images—essentially like templates for building new containers. 

Shared container libraries: Developers can access an open-source registry containing thousands of user-contributed containers. 
Today Docker containerization also works with Microsoft Windows and Apple MacOS. Developers can run Docker containers on any operating system, and most leading cloud providers, including Amazon Web Services (AWS), Microsoft Azure, and IBM Cloud offer specific services to help developers build, deploy and run applications containerized with Docker.

================
DOCKER COMMANDS
================
wsl: to start the docker, docker is up

// Install the mysql image if not present in the container list
docker run --detach --env MYSQL_ROOT_PASSWORD=dummypassword --env MYSQL_USER=social-media-user --env MYSQL_PASSWORD=dummypassword --env MYSQL_DATABASE=social-media-database --name mysql --publish 3306:3306 mysql:8-oracle

// Pause the container (application is running but will not work)
docker container pause <container-id>

// Unpause the containe
docker container unpause <container-id>

// inspect the container
docker container inspect <container-id>

// remove the stopped container
docker container prune 

// check if there is running container and which container is running currently
docker ps

// show all containers (including the non-running ones)
docker ps -a

// There may be a container with same name which might be installed, so you need to remove it first.
docker rm mysql(name of the container)

// to remove the image
docker rmi mysql(name of the container)

// to check the list of containers running
docker container ls

// to check the list of containers that are stopped or running
docker container ls -a

// to stop a particular docker
docker stop <container-id>

//docker executing that container:
docker exec -it <Container id> bash OR docker exec -it <Container id> sh
mysql -u social-media-user -p

// if you want to run the stopped container again 
docker start <container-id>

// displays all the locally cached images, which docker has downloaded from repository
docker images

// run the docker image on a speicific port
docker run -p 5000:5000 {Hostport}:{Containerport} image

// run the docker image in detache mode. Detaching the lifecycle of terminal with docker container
docker run -p 5000:5000 -d image

// run the docker image on a speicific port with restart value of always. Whenever the docker is up again, this container will start
docker run -p 5000:5000 {Hostport}:{Containerport}  -d --restart=always image

// if you want to see the logs from the container
docker logs <container-id>

// if you want to see the tailing logs from the container
docker logs -f <container-id> 

// you can tag an image with any number of tags
docker tag repo(image) tagname (docker tag in28min/rest-api:1.0.0 in28min/rest-api:latest)
Image id would be the same but it will be shown as two different images. Mostly while pulling, latest tag image is pulled from registry.

// docker has some official images hosted on docker hub like mysql, java
docker search mysql (it will search images having mysql in them)

// check the history of the docker image
docker image history <image id>

// remove the local image from docker
docker image remove <image id>

// docker events for all the activities happening in docker
docker events

// docker top shows the top process running in that particular container
docker top <container-id>

// docker top shows all the stats for the containers running
docker stats

// check what the docker demeons manager(Images, Containers, Local volume, Build cache)
docker system df

// building an docker image, you need to create docker file inside that project and write some commands. Last line has to be the entrypoint. docker build will build the image from that file and docker run will run that file using the last command in that file
docker build -t node-image:1 <image-name:tag>
docker run --name nodejs-container<name as u like> -p 80:3010 node-image:1 <image-name>

// login into your account
docker login

// push the image to docker repository
docker push anuraggarg/node-image-1 <username/image_name>

=======
As some microservices can be written in one language others can be written in another language. Deployment of each microservice seperately would be be very tedious and complex. How we can get one way of deploying Go, Java, Python, Javascript micoroservices? Containers.
And Docker is the best container tool. Create Docker images for each microservices. Docker image contains everything a microservice needs to run:
1) Application  (JDK or Python or NodeJS)
2) Application Code
3) Dependencies. 

One you have these, you can run these docker containers the same way on any infrasructure
- on local machine
- corporate data center
- cloud (AWS, Azure)

Docker is a software platform that allows you to build, test, and deploy applications quickly. Docker packages software into standardized units called containers that have everything the software needs to run including libraries, system tools, code, and runtime. Using Docker, you can quickly deploy and scale applications into any environment and know your code will run.

======
Docker works by providing a standard way to run your code. Docker is an operating system for containers. Similar to how a virtual machine virtualizes (removes the need to directly manage) server hardware, containers virtualize the operating system of a server. Docker is installed on each server and provides simple commands you can use to build, start, or stop containers. Using Docker lets you ship code faster, standardize application operations, seamlessly move code, and save money by improving resource utilization. With Docker, you get a single object that can reliably run anywhere. Docker's simple and straightforward syntax gives you full control. Wide adoption means there's a robust ecosystem of tools and off-the-shelf applications that are ready to use with Docker.

======
Docker is an open source platform that enables developers to build, deploy, run, update and manage containers—standardized, executable components that combine application source code with the operating system (OS) libraries and dependencies required to run that code in any environment. Containers simplify development and delivery of distributed applications. They have become increasingly popular as organizations shift to cloud-native development and hybrid multicloud environments. It’s possible for developers to create containers without Docker, by working directly with capabilities built into Linux and other operating systems. But Docker makes containerization faster, easier and safer.

======
Containers
Containers are made possible by process isolation and virtualization capabilities built into the Linux kernel. These capabilities—such as control groups (Cgroups) for allocating resources among processes, and namespaces for restricting a processes access or visibility into other resources or areas of the system—enable multiple application components to share the resources of a single instance of the host operating system in much the same way that a hypervisor enables multiple virtual machines (VMs) to share the CPU, memory and other resources of a single hardware server. 
As a result, container technology offers all the functionality and benefits of VMs—including application isolation, cost-effective scalability, and disposability—plus important additional advantages:
1) Lighter weight: Unlike VMs, containers don’t carry the payload of an entire OS instance and hypervisor. They include only the OS processes and dependencies necessary to execute the code. Container sizes are measured in megabytes (vs. gigabytes for some VMs), make better use of hardware capacity, and have faster startup times. 
2) Improved developer productivity: Containerized applications can be written once and run anywhere. And compared to VMs, containers are faster and easier to deploy, provision and restart. This makes them ideal for use in continuous integration and continuous delivery (CI/CD) pipelines and a better fit for development teams adopting Agile and DevOps practices.
3) Greater resource efficiency: With containers, developers can run several times as many copies of an application on the same hardware as they can using VMs. This can reduce cloud spending.

Companies using containers report other benefits including improved app quality, faster response to market changes and much more.

======
A dockerfile is a text document that contains all the commands a user call on the command line to assemble an image. 

======
Docker compose is a tool for defining and running multi-container Docker applications. With compose, you use a YAML file to configure your application's services. Then, with a single command, you create and start all the services from your configuration. Using compose is essentially a three-step process:
1) Define your app's environment with a Dockerfile so it can be reproduced anywhere.
2) Define the services that make up your app in docker-compose.yml, so that they can be run together in an isolated environment.
3) Run docker compose up and Docker compose command starts and runs your entire app.

======
Each container has its own: Root filesystem, processes, memory, devices, network ports. Containers share the host operating system kernal and make use of the guest operating system libraries for providing the required OS capabilities.

======
DOCKER IMAGES: A RECIPE OR TEMPLATE FOR CREATING DOCKER CONTAINERS. IT INCLUDES THE STEPS FOR INSTALLING AND RUNNING THE NECESSARY SOFTWARE

Docker Container: Like a tiny virtual machine that is created from the instructions found within the Docker image

Docker Client: Command-line utility or other tool that takes advantage of the Docker API (https://docs.docker.com/reference/api/docker_remote_api) to communicate with a Docker daemon

Docker Host: A physical or virtual machine that is running a Docker daemon and contains cached images as well as runnable containers created from images

Docker Registry: A repository of Docker images that can be used to create Docker containers. Docker Hub (https://hub.docker.com) is the most popular social example of a Docker repository.

Docker Machine: A utility for managing multiple Docker hosts, which can run locally in VirtualBox or remotely in a cloud hosting service such as Amazon Web Services, Microsoft Azure, Google Cloud Platform, or Digital Ocean.

======
The typical Docker workflow involves building an image from a Dockerfile, which has instructions on how to configure a container or pull an image from a Docker Registry such as Docker Hub. With the image in your Docker environment, you can run the image, which creates a container as a runtime environment with the operating systems, software, and configurations described by the image. For example, your result could be a container on the Debian operating system running a version of MySQL 5.5, which creates a specific database with users and tables required by your web application. These runnable containers can be started and stopped like starting and stopping a virtual machine or computer. If manual configurations or software installations are made, a container can then be committed to make a new image that can be later used to create containers from it. Finally, when you want to share an image with your team or the world, you can push your images to a Docker registry.

Docker file contains four commands, each of which creates a layer. Each layer is only a set of differences from the layer above it. The layers are stacked upon each other.

======
Pull Image From Docker Registry
The easiest way to get an image is to visit https://hub.docker.com and find an already prepared image to build a container from. There are many certified official accounts for common software such as MySQL, Node.js, Java, Nginx, or WordPress, but there are also hundreds of thousands of images created by ordinary people as well. If you find an image you want, such as mysql, execute the pull command to download the image.

docker pull mysql
If you don’t already have the image locally, Docker will download the most current version of that image from Docker Hub and cache the image locally. If you don’t want the current image and instead want a specific version, you can also use a tag to identified the desired version.

docker pull mysql:8.0.2
If you know you will want to run the image immediately after pulling it, you can save a step by just using the run command and it will automatically pull it in the background.

======	
If you are using Window 10 and are using docker toolbox
=> Use 192.168.99.100 instead of localhost. Note: If 192.168.99.100 does not work, you can find the IP by using the command docker-machine ip.
Reason: In Window 10 when using docker toolbox, docker is configured to use the default machine with IP 192.168.99.100

======
hub.docker.com: Registry contains a lot of repositories, a lot of different versions of different applications. This is public registry. Anyone can access this.
Image is static version hosted somewhere or donwloaded. But when it is run, it is run in container. Basically, Image is a class and Container be called as an object. For one image, there could be many containers. By default any container is run is part of something called BridgeNetwork in Docker. You can think of it as Internal Docker network. No one can excess it untill you specifically expose it onto the system. 
docker run -p 5000:5000(-p {HOST}:{CONTAINERPORT})

======
- If we want to kill the container just press cntrl+c. If we still want to keep the application running, use -d(detach mode) and will run in background.
docker run -p 5000:5000 -d repoName:1.0.0RELEASE
Id will be generated and whenever you want to see the logs you can use: docker logs id.
- docker images: will tell you the number of images donwloaded locally
- if you want to tail the logs use: docker logs -f id
- need to check the containers running: docker container ls
- running same image in new container: docker run -p 5001:5000 -d repoName:1.0.0RELEASE
- docker container ls -a: list of all the containers irrespective of their status
- stopping a container: docker container stop id

======
Docker architecture
					Docker Client(where we write the commands)
							|
					Docker Daemon/Engine
			/				|				\
	Docker Containers	Local Images	Image registry(nginx, mysql, eureka, your app)
	
When installed docker both Docker Client and Daemon are installed on client side.
Docker daemon responsible for managing the containers, images and pulling from registry. Also it helps in pushing the images to the registry.

======
Why docker is popular: We have just checked how to install the docker and run it locally. But it is also easy to do on cloud. Most of the cloud providers use container based services, run this container and it would run that container. Earlier their used to be two OS that need to be installed one of the host Os and second was the guest os and it would make it very heavy. But using docker, docker engine manages the guest os and all other software required to run the application. So it makes lightweight. All the service providers also coming up with the services around docker. Amazon provides: Elastic container service, azure provide azure container service.

======
Can create a new repo with same old repo with different tag name.
docker tag repo:1.0.0.RELEASE repo:latest
Most of the time you will see the release version latest is having the latest repo. But you need to always confirm before using it.
- docker pull repo : this will just download the repo from registry but won't run it.
- docker searchm mysql: this will search the image containing mysql name
- docker image inspect id: it will inspect and let you know the details of that image including entrypoint, volumes, tags.
- docker image remove id: it will remove from local machine, not from registry
There are official versions like sql, tomcat, java..

======
- docker run -p repo is actually docket container run -p repo, basically creating a new container.
- docker container pause container-id
- docker container unpause container-id
- docker container inspect container-id
- docker container ls -a: all the containers with their current status
- docker container prune: This will remove all the stopped containers
- docker container stop id: whenever we stop a container it will gracefully closes the container by closing the executer, dropping tables, shutting down services. SIGTERM
- docker container kill id: it will stop dead, no time was provided. SIGKILL.	
- docker run -p 5000:5000 -d -restart=always repoName:1.0.0RELEASE : if you stop the container, it will restart again when docker is restarted again. You need to prune the containers to stop getting it restarted.

======
- docker event: will let us know to see which container is stopped, started, disconnected
- docker top: it check which is the top process running in the container
docker top container-id
- docker stats: it shows all the stats which are running. Memory, CPU, Name, Limits
docker run -p 5000:5000 -m 512m --cpu-quota 5000 -d repoName:1.0.0RELEASE: here you are specifically telling the container not to use more then 512 memory and not more than 5000 quota of cpu-quota
- docker system df: tell all the different things that your docker deamon manages your images, containers

======
Zipkin: Distributed tracing: microservices have complex call chains. Need to debug problems. Trace the request across microservices. All the microservices including API gateway will send data to distributed tracing server and that data will be saved in the database. Zipkin is distributed tracing server that can be launched using docker container.

======
Monitoring vs observability: Monitoring is reactive(looks into logs, tracing). Observability is proactive(focus on what we understand what's happening in a system?).
Observability: - Gather data: metrics, logs, or traces 
- Get intelligence: AI/Ops and anomaly detection.

Monitoring is subset of observability.

OpenTelemetry: Collection of tools, APIs, SDKs to instrument, generate, collect, & export telemtry data(metrics, logs and traces). Having a standard for metrics, logs and traces. All cloud platforms provide support for OpenTelementry.

========================
Container Orchestration
=======================
Requirement: Want to run 10 instances of Microservice A container, 15 instances of B container:
Features:
- Auto scaling: Scale containers based on demand
- Service Discovery: Help microservices find one another
- Load balancer: Distribute load among multiple instances of a microservice.
- Self healing: Do health checks and replace failing instances
- Zero downtime deployments: release new versions without downtime

There are different container orchestration tools:
1) AWS specific
	- AWS Elastic container service (ECS)
	- AWS Fargate: Serverless version of AWS ECS
2) Cloud Neutral- Kubernets
	- AWS - Elastic Kubernetes Service (EKS)
	- Azure - Azure Kubernetes Service (AKS)
	- GCP - Google Kubernetes Engine (GKE)

======
Container orchestration is the automation of much of the operational effort required to run containerized workloads and services. Container orchestration automates the deployment, management, scaling, and networking of containers. This includes a wide range of things software teams need to manage a container’s lifecycle, including provisioning, deployment, scaling (up and down), networking, load balancing and more. Because containers are lightweight and ephemeral by nature, running them in production can quickly become a massive effort. Particularly when paired with microservices—which typically each run in their own containers—a containerized application might translate into operating hundreds or thousands of containers, especially when building and operating any large-scale system. This can introduce significant complexity if managed manually. Container orchestration is what makes that operational complexity manageable for development and operations—or DevOps—because it provides a declarative way of automating much of the work. This makes it a good fit for DevOps teams and culture, which typically strive to operate with much greater speed and agility than traditional software teams. Benefits:

======
What comes under container orchestration: Configuration, Provisioning, Availability, Scalability, Security, Resource allocation, Load balancing, Health monitoring

- Simplified operations: This is the most important benefit of container orchestration and the main reason for its adoption. Containers introduce a large amount of complexity that can quickly get out of control without container orchestration to manage it.
- Resilience: Container orchestration tools can automatically restart or scale a container or cluster, boosting resilience.
- Added security: Container orchestration’s automated approach helps keep containerized applications secure by reducing or eliminating the chance of human error. 

=================
Kubernetes : K8S
=================
Kubernetes manages resources. Resources are virtual servers. Different cloud providers have different name, AWS: EC2(ELastic compute cloud), Azure: Virtual machine, Google: Compute Engine, Kubernetes: Nodes.

=========
CLUSTERS
=========
A Kubernetes cluster is a set of nodes that run containerized applications. Containerizing applications packages an app with its dependences and some necessary services. They are more lightweight and flexible than virtual machines. In this way, Kubernetes clusters allow for applications to be more easily developed, moved and managed.

Kubernetes clusters allow containers to run across multiple machines and environments: virtual, physical, cloud-based, and on-premises. Kubernetes containers are not restricted to a specific operating system, unlike virtual machines. Instead, they are able to share operating systems and run anywhere.

Kubernetes clusters are comprised of one master node and a number of worker nodes. These nodes can either be physical computers or virtual machines, depending on the cluster.

The master node controls the state of the cluster; for example, which applications are running and their corresponding container images. The master node is the origin for all task assignments. It coordinates processes such as:
- Scheduling and scaling applications
- Maintaining a cluster’s state 
- Implementing updates 

The worker nodes are the components that run these applications. Worker nodes perform tasks assigned by the master node. They can either be virtual machines or physical computers, all operating as part of one system.  
There must be a minimum of one master node and one worker node for a Kubernetes cluster to be operational. 

========
kubectl
========
Its kube controller. It's a command to communicate with clusters. It will work irrespective of place, whether it is in local machine, physical machine, virtual machine. You want to deploy, increase the instances of the application, new version of application deployment. kubectl is already installed in cloud shell.

kubectl is single source of responsibility
when you run commands
kubectl create deployment ... -> it will create deployment, replicaset and pod
kubectl expose deployment... ->  it will create service

========================
KUBERNETES ARCHITECTURE
========================
Cluster is made up of Master node and worker nodes. 
As a master node most important part is ETCD

Master node:
1) API server: How google cloud talks to clusters. All the commands are run throgh this. Exposes a REST interface to all Kubernetes resources. Serves as the front end of the Kubernetes control plane. 
2) Scheduler: Places containers according to resource requirements and metrics. Makes note of Pods with no assigned node, and selects nodes for them to run on. 
3) Controller manager: Runs controller processes and reconciles the cluster’s actual state with its desired specifications. Manages controllers such as node controllers, endpoints controllers and replication controllers. Make sure desired state is maintained
4) Etcd: Its distributed database. All the configurations and deployments, scaling details are stored here. Stores all cluster data. Consistent and highly available Kubernetes backing store. Desired state is stored in this.

Worker node:
1) Node agent(Kubelet): Ensures that containers are running in a Pod by interacting with the Docker engine, the default program for creating and managing containers. Takes a set of provided PodSpecs and ensures that their corresponding containers are fully operational. Whatever happening on the node communicating back to the master node. If pod goes down, it will report to the master node. 
2) Networking component(Kube-proxy): Manages network connectivity and maintains network rules across nodes. Implements the Kubernetes Service concept across every node in a given cluster. Exposing service through deployement is possible through this only. Exposing services for pods. 
3) PODS: web application are run on this pods
4) Container runtime(CRI: docker, rkt, etc) 
 
These six components can each run on Linux or as Docker containers. The master node runs the API server, scheduler and controller manager, and the worker nodes run the kubelet and kube-proxy. 

======
Developer interaction uses the command line interface (kubectl) or leverages the API to directly interact with the cluster to manually set the desired state. kubectl with work with any cluster whether it is in local machine, remote, corporate server. it is used for deployment, increase instances, new version of application 

Deployoment: kubectl create deployment hello-world-rest-api --image=repoName:release
This repo is from docker registry. This will create deployment, replicaset and pod

expose deployment: kubectl expose deployment hello-world-rest-api --type=LoadBalancer --port=8080
This will expose the service to be used

=====
Pods
=====
kubectl get pods

A pod is the smallest execution unit in Kubernetes. A pod encapsulates one or more applications. Pods are ephemeral by nature, if a pod (or the node it executes on) fails, Kubernetes can automatically create a new replica of that pod to continue operations. Pods include one or more containers (such as Docker containers). A container can't be there without pod. Container lives inside a pod. Each pod has a unique IP address. Containers present in a pod share resources. Within the same pod, containers talk to each other using localhost. So, a kubernetes node contains multiple node and a single node can have mutliple containers.

Pods also provide environmental dependencies, including persistent storage volumes (storage that is permanent and available to all pods in the cluster) and configuration data needed to run the container(s) within the pod.

Pods represent the processes running on a cluster. By limiting pods to a single process, Kubernetes can report on the health of each process running in the cluster. Pods have:
1) a unique IP address (which allows them to communicate with each other)
2) persistent storage volumes (as required)
3) configuration information that determine how a container should run.
Although most pods contain a single container, many will have a few containers that work closely together to execute a desired function.

When pods contain multiple containers, communications, and data sharing between them is simplified. Since all containers in a pod share the same network namespace, they can locate each other and communicate via localhost. Pods can communicate with each other by using another pods IP address or by referencing a resource that resides in another pod.

Pods can include containers that run when the pod is started, say to perform initiation required before the application containers run. Additionally, pods simplify scalability, enabling replica pods to be created and shut down automatically based on changes in demand.

Containers need pods to be executed. Pod is a way to put your containers together, to give them an IP address and also provide a categorization of containers by associating them with labels.

==========
Namespace
==========
Namespace is very important. A pod by default is running in default namespace. It provides isolation from one part of cluster to other part of cluster. Ex. If we have two deployments, QA and Dev enviroments, keeping the resources separate for each environment. 

=====
Node
=====
A Pod always runs on a Node. A Node is a worker machine in Kubernetes and may be either a virtual or a physical machine, depending on the cluster. Each Node is managed by the control plane. A Node can have multiple pods, and the Kubernetes control plane automatically handles scheduling the pods across the Nodes in the cluster. The control plane's automatic scheduling takes into account the available resources on each Node.

Every Kubernetes Node runs at least:
Kubelet, a process responsible for communication between the Kubernetes control plane and the Node; it manages the Pods and the containers running on a machine.
A container runtime (like Docker) responsible for pulling the container image from a registry, unpacking the container, and running the application

A Kubernetes node is a single machine in a cluster that serves as an abstraction. Instead of managing specific physical or virtual machines, you can treat each node as pooled CPU and RAM resources on which you can run containerized workloads. When an application is deployed to the cluster, Kubernetes distributes the work across the nodes. Workloads can be moved seamlessly between nodes in the cluster.

Kubernetes Node Components.Here are the primary software components that run on every Kubernetes node:
1) kubelet: The kubelet is a software agent that runs on Kubernetes nodes and communicates with the cluster control plane. It allows the control plane to monitor the node, see what it is running, and deliver instructions to the container runtime. When Kubernetes wants to schedule a pod on a specific node, it sends the pod’s PodSecs to the kubelet. The kubelet reads the details of the containers specified in the PodSpecs, pulls the images from the registry and runs the containers. From that point onwards, the kubelet is responsible for ensuring these containers are healthy and maintaining them according to the declarative configuration.
2) kube-proxy enables networking on Kubernetes nodes, with network rules that allow communication between pods and entities outside the Kubernetes cluster. kube-proxy either forwards traffic directly or leverages the operating system packet filtering layer. kube-proxy can run in three different modes: iptables, ipvs, and userspace (a deprecated mode that is not recommended for use). iptables, the default mode, is suitable for clusters of moderate size, however it uses sequential network rules which can impact routing performance. ipvs can support a large number of services, as it supports parallel processing of network rules.
3) Container runtime: The container runtime, such as Docker, containerd, or CRI-O, is a software component responsible for running containers on the node.

===========
REPLICASET
===========
A ReplicaSet (RS) is a Kubernetes object used to maintain a stable set of replicated pods running within a cluster at any given time. A Kubernetes pod is a cluster deployment unit that typically contains one or more containers. Pods (and, by extension, containers) are, nevertheless, short-lived entities. Starting in the Pending phase, pods progress to the Running phase if at least one of their primary containers begins successfully. A container hosting a sample application, for example, could fail. As a host, you’d think the kubelet process would automatically recreate it, however pods aren’t automatically rescheduled when they die. This has an impact on the containers within it because they are not rescheduled and may be discarded owing to a lack of resources. To keep your application operating, you’ll need to keep track of the health of your pods.

The Kubernetes replication controller is in charge of ensuring that pod lifecycles are maintained by scheduling a replacement for any pods that fail. There are several distinct controllers, each having a different use case. They all have one thing in common: they all keep a watch on a specific cluster of pods to make sure the proper number of them are always running. In this article, we’ll focus on Replicasets (RS) as one of the key controllers and compare it to the others.

So what is a ReplicaSet ?
A ReplicaSet (RS) is a Kubernetes object used to maintain a stable set of replicated pods running within a cluster at any given time. 

As stated above, the main goal of kubernetes controllers is to define a desired state, by ensuring at any given moment, N pods of the same type are constantly running at all times. As such, RS are often used to guarantee the availability of a service. Kubernetes will automatically deploy extra pods to replace those that fail or become inaccessible, preventing users from losing access to an application. If we didn’t use ReplicaSets, we would have to build as many manifests as the number of pods we require, which would be a lot of work to do for each application. 

A ReplicaSet has two main features: a pod template for creating new pods whenever existing ones fail, and a replica count for maintaining the desired number of replicas that the controller is supposed to keep running. A ReplicaSet also works to ensure additional pods are scaled down or deleted whenever an instance with the same label is created

kubectl get replicasets (kubectl get rs)
Replicasets ensure a particular number of pods are running all the time. If a node is deleted, the replicaset will create a new pod instantly and service would keep on running.
Whenever you want to tie a pod with replicaset or service, labels are used.  

===========
Deployment
===========
A Kubernetes Deployment tells Kubernetes how to create or modify instances of the pods that hold a containerized application. Deployments can help to efficiently scale the number of replica pods, enable the rollout of updated code in a controlled manner, or roll back to an earlier deployment version if necessary. Kubernetes deployments are completed using kubectl, the command-line tool that can be installed on various platforms, including Linux, macOS, and Windows. 

Kubernetes saves time and mitigates errors by automating the work and repetitive manual functions involved in deploying, scaling, and updating applications in production. Since the Kubernetes deployment controller continuously monitors the health of pods and nodes, it can make changes in real-time—like replacing a failed pod or bypassing down nodes—to ensure the continuity of critical applications.

Deployments automate the launching of pod instances and ensure they are running as defined across all the nodes in the Kubernetes cluster. More automation translates to faster deployments with fewer errors.

If a new deployment fails, it will keep on running the old deployment. 
Deployment-> Mutliple Replicaset -> Multiple Pods

Deployment used 2 types of strategies:
1) Re-create: Destroy all pods of v1 at same time and then create new images of v2. Disadvantage: if v2 is invalid, v1 will be destroyed and v2 will not run as it is invalid.
2) ROlling updates: Once the v2 pod starts running then only one v1 pod is destroyed. It also helps if we want to deploy 50% of the pods with new version. 

=========
SERVICES
=========
In Kubernetes, a Service is a method for exposing a network application that is running as one or more Pods in your cluster.

A key aim of Services in Kubernetes is that you don't need to modify your existing application to use an unfamiliar service discovery mechanism. You can run code in Pods, whether this is a code designed for a cloud-native world, or an older app you've containerized. You use a Service to make that set of Pods available on the network so that clients can interact with it. If you use a Deployment to run your app, that Deployment can create and destroy Pods dynamically. From one moment to the next, you don't know how many of those Pods are working and healthy; you might not even know what those healthy Pods are named. Kubernetes Pods are created and destroyed to match the desired state of your cluster.Each Pod gets its own IP address (Kubernetes expects network plugins to ensure this). For a given Deployment in your cluster, the set of Pods running in one moment in time could be different from the set of Pods running that application a moment later. This leads to a problem: if some set of Pods (call them "backends") provides functionality to other Pods (call them "frontends") inside your cluster, how do the frontends find out and keep track of which IP address to connect to, so that the frontend can use the backend part of the workload?
Enter Services.

======
The Service API, part of Kubernetes, is an abstraction to help you expose groups of Pods over a network. Each Service object defines a logical set of endpoints (usually these endpoints are Pods) along with a policy about how to make those pods accessible.

For example, consider a stateless image-processing backend which is running with 3 replicas. Those replicas are fungible—frontends do not care which backend they use. While the actual Pods that compose the backend set may change, the frontend clients should not need to be aware of that, nor should they need to keep track of the set of backends themselves.

A Kubernetes service is a logical abstraction for a deployed group of pods in a cluster (which all perform the same function). Since pods are ephemeral, a service enables a group of pods, which provide specific functions (web services, image processing, etc.) to be assigned a name and unique IP address (clusterIP). As long as the service is running that IP address, it will not change. Services also define policies for their access.
======
There are four types of Kubernetes services — ClusterIP, NodePort, LoadBalancer and ExternalName. The type property in the Service's spec determines how the service is exposed to the network.
- ClusterIP: Exposes a service which is only accessible from within the cluster.
- NodePort: Exposes a service via a static port on each node’s IP.
- LoadBalancer: Exposes the service via the cloud provider’s load balancer.
- ExternalName: Maps a service to a predefined externalName field by returning a value for the CNAME record.

======
Nodeport: Expose a service via a static port on each node's IP. Nodeparts are open ports on every cluster node. K8s will route traffic that comes into a Nodeport to the service. NodePort is intended as a foundation for other higher-level methods of ingress such as load balancers and are useful in development.
targetPort: 80
port: 80
nodePort: 30080 (Need to enable port on GCP firewall)

======
Cluster IP Service
Expose a service which is only accessible from whitin the cluster. It is default type of service, which is used to expose a service on an IP address internal to the cluster. Access is only permitted from within the cluster.

======
Loadbalancer Service
It is used by client to hit. It will loadbalance itself for the nodes running. It will create Nord port service and uses it for load balancing.

======
Kubernetes services enable communication between various components within and outside of the application. It helps us connect applications together with other applications or users.

============
CONFIG MAPS
============
Configmaps provide a way to store configuration information and provide it to containers. They can be used to store fine-grained or coarse-grained configuration
- indiviual properties
- entire configuration file
- JSON files
ConfigMaps hold configuration in key-value pairs accessible to Pods.

apiVersion: v1
kind: ConfigMap
metadata:
  name: special-config
  namespace: default
data:
  special.how: very
  special.type: charm
  
In place of doing like this just mention the other way around. It will directly show the key value pair as environment variable for the app

========
SECRETS
========
Secret is an object that contains small amount of sensitive data such as a password, a token, or a key. Secret reduces the risk of exposing sensitive data to unwanted entities. Like ConfigMaps, Secrets are Kubernetes API objects created outside of pods. Secrets belong a specific Kubernetes Namespace. Secrets are not entirely foolproof. API server stores secrets in plain text. During replication across etcd clusters, Secrets are send in plain text. Secret definitions may still get exposed to outside world.

It is recommended to run the command to create the secret in place of having the yaml file, as it may get stored on git which may expose it to outside world.

======
Five types of secrets:
1) Generic secrets:  create secret from local file, directory or literal value. 
2) TLS secret from given public/private key pair. The public/private key pair must exist beforehand. The public key certificate must be .PEM encoded and match the given private key.
Some more are there.. not in foucs though

apiVersion: v1
kind: Secret
metadata:
  namespace: gagan-k8s<namespace>
  name: secret-info
data:
  DB_PASSWORD: bcXlxmjd7f5dhdi9==
  USER_PASSOWRD:  bcXlxmjd7f5dhdi9==
  
=========================
Horizontal pod autoscale
=========================
Increase number of Pods for a workload depending on certain metrics. Metrics can be CPU usage, Memory usage. If that threshold is met, it will scale the number of replicas (deployments).

scaling using command:
kubectl autoscale deployment nginx --cpu-precent=50 --min=1 --max10

=============
VOLUME MOUNT
=============
Volume mount is basically getting the persistence storage from the node to be mounted on the pod. So if there is file structure present on the node, you can mount it to the pod. So whatever has been written on the node file structure will be available on pod and vice versa.

So the pod having volume mount configuration, will use the node to which it has been assigned. If we have many instances of the pod that are mounting the volume, they will  be using the same physical storage path.

Disadvantage: You need to mention the host volumes again and again for each pod generated. You won't be able to manage the volumes.

===================
PERSISTENT VOLUMES
===================
You will have large storage kubernetes administrator can create persistence volumes of different capacity. FOr a pod to use a persistent volume, PVC is created (persistent volume claim) depending upon capacity requirement. If one perisitent volume is claimed then it won't be used by another claim. Its one to one mapping.
 
Claim is part of cluster, not related to node. Volume is of cluster, but underlying storage is of one node. 
We generally use the shared path storage in place of node storage. 

================
Storage classes
================
In many cases, it is not possible to create physical storage beforehand. Storage should be created automatically for you on runtime. There is a provisioner whose responsibility is to provision that storage for you.  

==============
STATEFUL SETS
==============
When we do a deployment with 3 replicas, no restriction on name, not on time, they can come up anytime. With stateful sets it creates in order not in random ids. Basically used by databases. In DB, there is one master node and another worker node. 

==============
MORE COMMANDS
==============
kubectl autoscale deployment hello-world-rest-api --max=10 --cpu-percent=70
kubectl edit deployment hello-world-rest-api #minReadySeconds: 15

// if we are pulling the yaml file into the gc console and then run the following command
kubectl apply -f nginx-pod.yaml<file-name>

// details, whether that pod is up and the logs associated with it
kubectl describe pod nginx-pod<name-of-the-pod>

// to resolve the issue this command is important 
gcloud container clusters get-credentials in28minutes-cluster --zone us-central1-a --project solid-course-258105

kubectl get pods --all-namespaces
 
// get details about pods
kubectl get pods -o wide

// get events : list of all the events like creating of pod, starting node
kubectl get events

// running pods
kubectl get pods
kubectl get po

// get replicasets 
kubectl get replicaset

// get all the deployments
kubectl get deployment
kubectl get deploy

// get all the services
kubectl get service
kubectl get svc
kubectl get svc --watch
 
// delete pod
kubectl delete pod <name-of-pod>
 
//kubectl explain pods gives information about the pods
kubectl get pods -o wide
 
// show all the replicasets  
kubectl get replicasets
kubectl get replicaset
kubectl get rs
kubectl get rs -o wide
 
// delete all workloads, services, etc. But not the cluster 
kubectl delete all --all
 
// increase the deployment(number of nodes) to 3.
kubectl scale deployment hello-world-rest-api --replicas=3

// delete a replicaset
kubectl delete rs <name-of-replicaset>

// get and show the events in sorted order
kubectl get events --sort-by=.metadata.creationTimestamp
 
// setting the deployment to new release
kubectl set image deployment hello-world-rest-api hello-world-rest-api=DUMMY_IMAGE:TEST
kubectl set image deployment <name-of-deployment> <container-name> <new-image-name>

// get the health of the components 
kubectl get componentstatuses

// creating a secret
kubectl create secret <type> <name> <data>

// get persistent volumes
kubectl get pv

// get persistent volumes claims
kubectl get pvc


kubectl get all
kubectl create deployment currency-conversion --image=in28min/mmv2-currency-conversion-service:0.0.11-SNAPSHOT
kubectl expose deployment currency-conversion --type=LoadBalancer --port=8100
kubectl get deployment currency-exchange -o yaml >> deployment.yaml 
kubectl get service currency-exchange -o yaml >> service.yaml 
kubectl diff -f deployment.yaml
kubectl apply -f deployment.yaml
kubectl delete all -l app=currency-exchange
kubectl delete all -l app=currency-conversion
kubectl rollout history deployment currency-conversion
kubectl rollout history deployment currency-exchange
kubectl rollout undo deployment currency-exchange --to-revision=1
kubectl logs currency-exchange-9fc6f979b-2gmn8
kubectl logs -f currency-exchange-9fc6f979b-2gmn8 
kubectl autoscale deployment currency-exchange --min=1 --max=3 --cpu-percent=5 
kubectl get hpa
kubectl top nodes
kubectl create configmap currency-conversion --from-literal=CURRENCY_EXCHANGE_URI=http://currency-exchange
kubectl get configmap
kubectl get configmap currency-conversion -o yaml >> configmap.yaml
watch -n 0.1 curl http://34.66.241.150:8100/currency-conversion-feign/from/USD/to/INR/quantity/10
