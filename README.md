Changing the Read-Me

# hello-world
Checking recommended github setup

https://developers.redhat.com/blog/2017/07/10/using-opentracing-with-jaeger-to-collect-application-metrics-in-kubernetes/

https://www.hawkular.org/blog/2017/06/9/opentracing-spring-boot.html

https://github.com/pavolloffay/opentracing-java-examples/blob/master/spring-boot/pom.xml

https://github.com/opentracing-contrib/java-spring-web

https://developers.redhat.com/blog/2019/05/01/a-guide-to-the-open-source-distributed-tracing-landscape/

https://www.w3.org/TR/trace-context/#trace-id

https://shekhargulati.com/2019/04/08/a-minimalistic-guide-to-distributed-tracing-with-opentracing-and-jaeger/

https://github.com/opentracing-contrib/java-spring-cloud

#######################################################################################################################################################
Background

From first part of blog we gathered understanding about basics of Chaos Engineering. Now we will further deep dive to understand how to perform Chaos Engineering with a working example - which to me is going to be quite interesting. First lets start with understanding basics of working example which will be used to demonstrate following-

    How to perform chaos engineering within an application
    How to monitor the behavior of the system
    What to monitor whilst executing experiments on our system

Bird's eye view of demo example

Demo App - Architecture Overview

Demo App - Architecture Overview

The working example (along with its source code) which we will be using for demonstration, primarily consists of 2 simple Spring Boot applications -

    Card Client - Public facing edge application
    Card Service - Application which has core domain of card. Since it owns business workflow, it will be using  Redis as persistent store. It also has a bulk loader which will load 5000 cards into the database whenever it bootstraps.

To keep things simple, both the applications will just expose 2 APIs

    Get Card by Id - Returns a card for the corresponding id
    Get All Cards - Returns list of 5000 cards

Since Card Service is a core domain application, we will be quite curious to know how will Card Client behave if

    Card Service is either down
    Card Service is responding too slowly

Hence we will be performing chaos engineering experiments with Card Service application as blast radius.

As per my earlier post, most fundamental prerequisite for performing Chaos Engineering experiments is to have state of the art Monitoring available in order to understand system behavior by capturing required metrics. This is the key as it will help in identifying vulnerable areas of system which needs to be further hardened. So in order to enable required monitoring stack we will need some additional infrastructure components as mentioned below -

    Consul - It is a service mesh solution providing features like service discovery, health check, secure service communication etc
    Prometheus - It is an open source monitoring and alerting tool built by SoundCloud
    Grafana - It is a visualizing tool that allows us to view different types of metrics by formulating queries and alerts. It has an excellent looking aesthetic UI.

Note - You can refer to my older blog  for understanding how Spring Boot's Actuator, Micrometer,  Prometheus and Grafana works in unison to provide us required system metrics.

In order to generate chaos we will inject failures and for that we will make use of some tool. As on date there are myriad tools available in market viz. Chaos IQ, Gremlin, Simian Army etc. Since we need to inject failures within application, we will be using Chaos Monkey for Spring Boot (CM4SB)
Internals of Chaos Monkey for Spring Boot (CM4SB)

At a high level Chaos monkey for Spring Boot basically consists of **Watchers **and Assaults.
Watchers

It will basically scan Spring Boot app for specific annotation (as per the configured values). It supports all the Spring annotation -

    @Controllers
    @RestControllers
    @Service
    @Repository
    @Component

By using AOP, CM4SB will identify the public method on which configured assaults need to be applied. One can even customize behavior of Watcher by using _watchedCustomService _property and thereby decide which classes and their public methods need to be assaulted
Assaults

They are the most important component of CM4SB. They are basically categorized into -

    Latency Assault - Adds latency to the request. Number of requests can be controlled by level
    Exception Assault - Enables throwing of RuntimeException as per the configured value
    Appkiller Assault - Shuts down the application. The only caveat with this assault is, once the application is shut down, it needs manual step to restart the application.

Metrics Emitted by CM4SB
Type of Metric	Metric name
Chaos Monkey metric request count	chaos_monkey_application_request_count_total chaos_monkey_application_request_count_assaulted chaos_monkey_assault_component_watcher_total chaos_monkey_assault_controller_watcher_total chaos_monkey_assault_repository_watcher_total chaos_monkey_assault_restController_watcher_total chaos_monkey_assault_service_watcher_total
Chaos Monkey metric latency count in ms	chaos_monkey_assault_latency_count_gauge chaos_monkey_assault_latency_count_total
Chaos Monkey metric exception	chaos_monkey_assault_exception_count

Note - We will be viewing each of this metric via Grafana dashboard as we perform chaos engineering experiments with our working example
Key implementation aspects
1. Adding Maven Dependency

1<dependency>
2	<groupId>de.codecentric</groupId>
3	<artifactId>chaos-monkey-spring-boot</artifactId>
4	<version>2.0.2</version>
5</dependency>

###############################################################################################

Title: Abstract: Developer API Store in Enterprise Architecture

In today’s digital landscape, enterprise architecture is increasingly reliant on APIs (Application Programming Interfaces) to enable seamless communication and integration between various systems and services. The Developer API Store serves as a centralized platform within the enterprise architecture, offering a structured approach to API management and consumption. This abstract delves into the key components and benefits of integrating a Developer API Store into enterprise architecture.

	1.	Overview of Developer API Store: The abstract outlines the core functionality of a Developer API Store, emphasizing its role as a repository for APIs developed internally or sourced externally. It highlights features such as API discovery, documentation, versioning, and security protocols.
	2.	Integration within Enterprise Architecture: Discusses how the Developer API Store fits into the broader enterprise architecture framework, including integration with existing systems, data sources, and application workflows. Emphasis is placed on creating a scalable and interoperable ecosystem.
	3.	Governance and Lifecycle Management: Explores governance models and lifecycle management strategies within the Developer API Store. This includes access controls, API usage policies, monitoring, and analytics to ensure compliance, security, and optimal performance.
	4.	Developer Experience and Collaboration: Highlights the importance of providing a seamless and intuitive developer experience within the API Store, including self-service capabilities, sandbox environments, and collaboration tools to foster innovation and accelerate development cycles.
	5.	Business Impact and ROI: Addresses the tangible business benefits of implementing a Developer API Store, such as faster time-to-market for new services, improved partner integration, enhanced customer experiences, and potential revenue streams through API monetization.
	6.	Future Trends and Adaptability: Considers emerging trends in API management, such as AI-driven API analytics, microservices architecture, and cloud-native deployments, and discusses how the Developer API Store can evolve to accommodate these changes.

By examining these key aspects, this abstract provides a comprehensive understanding of the Developer API Store’s role in modern enterprise architecture, its impact on digital transformation initiatives, and strategies for maximizing its value within an organization.

###############################################################################################

Title: Leveraging Machine Learning for Public Cloud Spend Optimization

Abstract:
The widespread adoption of public cloud services has led to increased complexities in managing and optimizing cloud spend for organizations. This abstract explores the application of machine learning (ML) techniques in the realm of public cloud spend optimization. By leveraging ML algorithms, businesses can analyze historical spending patterns, forecast future usage, identify cost optimization opportunities, and automate decision-making processes to ensure efficient resource allocation and cost-effective cloud utilization. This abstract delves into various ML models such as anomaly detection, predictive analytics, and optimization algorithms that are instrumental in achieving significant cost savings while maintaining performance and scalability in cloud environments. Case studies and examples are provided to illustrate the practical implementation and benefits of ML-driven cloud spend optimization strategies.




	
		

	


	
		

	


	
		

	


	
		

	

































	
		

	


	
		

	


	
		

	


	
		

	


	
		

	


	
		

	































java
2. Activate Chaos Monkey for Spring Boot and Watcher related properties within application configurations

 1spring:
 2  profiles:
 3    active: chaos-monkey
 4
 5chaos:
 6  monkey:
 7    watcher:
 8      component: false
 9      controller: false
10      repository: false
11      rest-controller: true
12      service: true

...


	
		

	


	
		

	


	
		

	


	
		

	

































	
		

	


	
		

	


	
		

	


	
		

	


	
		

	


	
		

	































java
3. Enabling Chaos Monkey endpoints for monitoring

 1management:
 2  endpoints:
 3    web:
 4      exposure:
 5        include: \["\*"\]
 6  #        include: \["info", "health", "prometheus", "chaosmonkey"\]
 7  metrics:
 8    tags:
 9      application: ${spring.application.name}
10    distribution:
11      percentiles:
12        http.server.requests: 0.5, 0.9, 0.95, 0.99
13
14  endpoint:
15    chaosmonkey:
16      enabled: true

...


	
		

	


	
		

	


	
		

	


	
		

	

































	
		

	


	
		

	


	
		

	


	
		

	


	
		

	


	
		

	































java
List of HTTP Endpoints
HTTP URI	Description	HTTP Method
/chaosmonkey	Running Chaos Monkey configuration	GET
/chaosmonkey/status	Is Chaos Monkey enabled or disabled?	GET
/chaosmonkey/enable	Enable Chaos Monkey	POST
/chaosmonkey/disable	Disable Chaos Monkey	POST
/chaosmonkey/watcher	Running Watcher configuration. NOTE: Watcher cannot be changed at runtime, they are Spring AOP components that have to be created when the application starts.	GET
/chaosmonkey/assaults	Running Assaults configuration	GET
/chaosmonkey/assaults	Change Assaults configuration	POST

 
Demonstration of working example

Once we have our both the applications i.e. Card Client and Card Service along with Consul, Prometheus and Grafana up and running, we can inject failure using CM4SB. For monitoring CM4SB metrics, we have imported Grafana Dashboard with id as 9845. Before initiating chaos engineering experiments lets understand current configurations of Chaos Monkey by invoking '/actuator/chaosmonkey' with HTTP GET method

 1{
 2    "chaosMonkeyProperties": {
 3        "enabled": false
 4    },
 5    "assaultProperties": {
 6        "level": 5,
 7        "latencyRangeStart": 1000,
 8        "latencyRangeEnd": 3000,
 9        "latencyActive": true,
10        "exceptionsActive": false,
11        "exception": {},
12        "killApplicationActive": false,
13        "frozen": false,
14        "proxyTargetClass": true,
15        "proxiedInterfaces": \[\],
16        "preFiltered": false,
17        "advisors": \[
18            {
19                "order": 2147483647,
20                "advice": {},
21                "pointcut": {
22                    "classFilter": {},
23                    "methodMatcher": {
24                        "runtime": false
25                    }
26                },
27                "perInstance": true
28            }
29        \],
30        "targetSource": {
31            "target": {
32                "level": 5,
33                "latencyRangeStart": 1000,
34                "latencyRangeEnd": 3000,
35                "latencyActive": true,
36                "exceptionsActive": false,
37                "exception": {},
38                "killApplicationActive": false
39            },
40            "static": true,
41            "targetClass": "de.codecentric.spring.boot.chaos.monkey.configuration.AssaultProperties"
42        },
43        "exposeProxy": false,
44        "targetClass": "de.codecentric.spring.boot.chaos.monkey.configuration.AssaultProperties"
45    },
46    "watcherProperties": {
47        "controller": false,
48        "restController": true,
49        "service": true,
50        "repository": false,
51        "component": false
52    }
53}

...


	
		

	


	
		

	


	
		

	


	
		

	

































	
		

	


	
		

	


	
		

	


	
		

	


	
		

	


	
		

	































java

As we can see from above response, it mainly depicts key configurations pertaining to Chaos Monkey for the corresponding Spring Boot application -

    Is Chaos Monkey for Spring Boot enabled
    Assault Configurations for Latency, Exception and Kill Application
    Watcher Configurations which mainly indicates Spring annotations on which configured assaults will be applied

Next we need to enable Chaos Monkey. So we will be using '/actuator/chaosmonkey/enable' URI. As soon as it is enabled, we can clearly see it in Grafana dashboard (under 'Chaos Monkey Status') as shown below

First Chaos Experiment

As part of the first experiment, we will be generating chaos by applying Latency Assault via '/actuator/chaosmonkey/assaults' URI. Latency induced will be ranging from 4 - 7 seconds with level configured as 5. Request payload for configuring assault related settings will be as shown below

 1{
 2"level": 5,
 3"latencyRangeStart": 4000,
 4"latencyRangeEnd": 7000,
 5"latencyActive": true,
 6"exceptionsActive": false,
 7"killApplicationActive": false,
 8"exception": {
 9    "type": "java.lang.IllegalArgumentException",
10    "arguments": \[{
11      "className": "java.lang.String",
12      "value": "custom illegal argument exception"}\] }
13}

...


	
		

	


	
		

	


	
		

	


	
		

	

































	
		

	


	
		

	


	
		

	


	
		

	


	
		

	


	
		

	































java

After running 250 requests with 4 concurrent threads for fetching all cards using Apache Bench

1> ./ab.exe -n 250 -c 4 http://localhost:8090/cards



	
		

	


	
		

	


	
		

	


	
		

	

































	
		

	


	
		

	


	
		

	


	
		

	


	
		

	


	
		

	































we can clearly see below metrics within the imported dashboard

    Total number of incoming requests for Card Service application (as per selected time frame)
    Total number of requests for which latency was induced
    Actual latency induced (in seconds)

Since we have also configured distribution percentiles for server request we can also see metrics pertaining to response time. As we can see from below metrics, that response time of Get all Cards API is taking 7 seconds.

Second Chaos Experiment

As part of this experiment we will be applying Exception Assault to understand Card Client behavior in case Card Service ends up with Runtime exceptions. So we will modify the assault configuration by applying below request payload with same URI as above i.e. '/actuator/chaosmonkey/assaults'

 1{
 2"level": 5,
 3"latencyRangeStart": 4000,
 4"latencyRangeEnd": 6000,
 5"latencyActive": false,
 6"exceptionsActive": true,
 7"killApplicationActive": false,
 8"exception": {
 9    "type": "java.lang.IllegalArgumentException",
10    "arguments": \[{
11      "className": "java.lang.String",
12      "value": "custom illegal argument exception"}\] }
13}

...


	
		

	


	
		

	


	
		

	


	
		

	

































	
		

	


	
		

	


	
		

	


	
		

	


	
		

	


	
		

	































java

After running the same tests (i.e. Fetch all Cards using the same above Apache Bench command) we can clearly see metrics within Exception Section of dashboard (as shown below). What we are able to see here is

    Total number of requests received by Card Service app
    Number of exceptions thrown
    Error Rate

Within HTTP code section of dashboard we can also see number of requests that have ended up with HTTP code as 500 (due to Runtime Exception thrown by CM4SB assault)

Third Chaos Experiment

As part of this chaos experiment we will be applying kill application Assault to understand Card Client behavior in case Card Service inadvertently goes down. So we will change the assault configuration by applying below request payload with same URI as above i.e. '/actuator/chaosmonkey/assaults'

 1{
 2     "level": 5,
 3     "latencyRangeStart": 4000,
 4     "latencyRangeEnd": 6000,
 5     "latencyActive": false,
 6     "exceptionsActive": false,
 7     "killApplicationActive": true, 
 8     "exception": { 
 9          "type": "java.lang.IllegalArgumentException", 
10          "arguments": \[{ 
11               "className": "java.lang.String", 
12               "value": "custom illegal argument exception"
13          }\] 
14     }
15}

...


	
		

	


	
		

	


	
		

	


	
		

	

































	
		

	


	
		

	


	
		

	


	
		

	


	
		

	


	
		

	































java

After running the same tests (i.e. Fetch all Cards using the same above Apache Bench command) we can clearly see that there is not a single failure ! So we can infer that our fallback mechanism that we have configured within Card Client application is working as per its expected behavior in case Card Service becomes unresponsive.

Conclusion

With this 2 part series on Chaos Engineering we were able to understand the fundamentals of Chaos Engineering along with following points -

    Significance of Chaos Engineering in this distributed world
    How to perform chaos in Spring Boot applications
    How to monitor application behavior and derive metrics from it

Due to  inevitability of failures in software world, I am sure that you are convinced about the need of a disciplined and an organized way to proactively test system by injecting failures - that discipline is none other than CHAOS ENGINEERING! I would like to end this 2 part series with an apt and famous axiom :

    The MORE you SWEAT in PEACE,

    the LESS you BLEED in WAR
    — Norman Schwarzkopf

which completely resonates with 'Why Chaos Engineering' should be formally inducted as part of (distributed) application development and its production release
##############################################################################################################################################

Human body is vulnerable to lot of diseases. So in order to protect human beings from diseases vaccines have been invented. Vaccines mainly work due to process called 'Hormesis', by which system or organism adapts to harm in order to become stronger. Just as our body is susceptible to diseases and germs, so do our systems in software world. Hence vaccines and vaccination can be considered as an apt analogy for understanding Chaos Engineering. So as two part series on Chaos Engineering, we will try understanding the basics of Chaos Engineering in first part which will then be followed up by demonstrating working example in the second and final part of this series
What is Chaos Engineering?

Theoretical definition from Principles of Chaos

    "Chaos Engineering is the discipline of experimenting on a system in order to build confidence in the system's capability to withstand turbulent conditions in production"

So in layman's term it is thoughtful, planned experiments executed in organized way to reveal weaknesses in software system. The whole purpose of being thoughtful, planned and organized is to be able to understand impact of experiments on system and thereby monitor system behavior. It is a scientific approach by which hypothesis about our systems are validated, which eventually helps us to discover significant information about our systems.

Just as different categories of tests help us build confidence in application, Chaos Engineering is a proactive way of validating our systems. Of course one would argue that we can still validate our systems by following the conventional and reactive approach to our operations during production incidents or outages. But this reactive approach will never get us to degree of reliability that we intend to.
Misconceptions

It is certainly not a lone wolf game where in one of the team members becomes a cow boy and runs into our system with all guns blazing pointed to our servers, database, infrastructure etc. Chaos engineering provides a platform to collaborate and thereby test communication boundaries - which not only includes technical but also include socio-technical boundaries. So it is certainly not to be done in a secretive fashion, just like James Bond's movies ;)
Why Chaos Engineering?
1. Potential Failure Points of System

Typically speaking, system can be mainly categorized into -

    Application Tier
    Caching Layer
    Database Tier
    Network
    Hardware
    Tools which are used for managing and operating software systems viz. Skype, Splunk, Self serving portals etc.

Most important thing to notice here is that all the above categories are not just falling in developer realm or QA realm or Devops realm. All of them will have to get together in order to build reliability into systems. All the above categories of system are vulnerable to failures. However, degree of vulnerability may vary.
2. Shift in Architecture paradigm

In today's contemporary world of software systems, most of the green field projects are built with Cloud Native Mircroservice architecture. And brown field projects are undergoing a major transformation i.e. moving from Monolithic architecture to Microservice architecture. With Microservice architecture there are lot of disconnected and moving pieces within the system. Now if we consider this shift in architecture paradigm through the lens of above failure points, there would be almost 6 * N failure points to deal with, where N is number of Microservice applications for a given system - needless to say that answer to this multiplication will look too substantial to ignore!

As per Murphy's law, failure in software systems is inevitable. So Chaos Engineering in a way helps to identify vulnerable areas within our system and thereby help us to either get rid of failures or reduce MTTR

Netflix, one of the pioneers of Chaos Engineering gave an excellent quote

    Best way to avoid failure is to fail constantly

So Chaos Engineering allows to inject failures into the systems, that too on regular basis so that we can constantly identify weaknesses in our system and thereby harden them so that we are better prepared for production incidents or outages.
3. Predictability

Another compelling reason why Chaos Engineering should be performed is to have more predictability in our systems. In contemporary world our systems have become too disconnected. Variables in our system are myriad i.e. these variables may belong to user, infrastructure, network, hardware etc. and since we are unable to know all the variables within our system, we can not model and build system considering them - E.g. With public cloud based strategy we may not even own our hardware. So in order to bring more predictability, we need a mechanism to understand these variables of our system and thereby be better equipped for those unwanted and abrupt production incidents and outages
How to perform Chaos Engineering
1. Monitoring - An important prerequisite

One of the most important prerequisite to perform Chaos Engineering is to have state of the art Monitoring. It is advisable to refrain performing Chaos Engineering experiments if system does not have appropriate monitoring in place; as we may not be able to gather required system metrics that will help us to identify vulnerabilities and weaknesses which will eventually lead us to actionable insights
2. Game Day Planning

Basically chaos engineering will start with Planning a Game Day wherein we will have representatives from owners of the system (i.e. application, database, network, hardware, tools etc). Basic premise of this ceremony is to answer below questions -

    From system standpoint, what can go wrong or how can system fail?
    How to meticulously plan and effectively execute tests pertaining to answers that we get for above question?

So as part of 1st question one should be coming up with application's system and infrastructure view. This will in a way help us to understand probable failure points within the system. Above image just represents infrastructure view of a typical Microservice application with a hypothetical scenario

    Factors to be considered for identified probable failure points Now that we know how chaos engineering experiments should be executed, it is quite obvious that there can be zillions of failure points that can emerge as part of game day planning. But we may not want to perform chaos engineering experiments for all of them. There are 2 primary criteria which helps us decide on consideration of failure points for performing chaos engineering experiments -

        Probability of its occurrence - Fundamental question that needs to be asked is how likely a failure can occur. It may be that certain failures are least likely. However, we may still want to be extremely sensitive to such rare failures by checking whether it can lead to black swan event and thereby have a catastrophic impact on organization

        Cost of failure - Understanding cost of failure will in a way help to justify the effort spent for performing Chaos Engineering experiments in front of the organization's leadership.

    Chaos Engineering Workflow

Chaos engineering has a very important fundamental that always needs to be kept in mind i.e. Blast Radius - which is nothing but the impacted area of system as an outcome of the chaos engineering experiments. In principle it should be as small as it can be so that chaos engineering experiments does not have a humongous impact, which is extremely complex and difficult to understand and analyze. So we should strive to run smallest possible experiment that can teach us something about our system's behavior.

 

Similar to scientific experiments, Chaos Engineering typically starts with a hypothesis built around our system. And then we have set of experiments to validate our hypothesis. There are  probable outcomes to our validation phase - Failure or Successful.

If validation of hypothesis ends up in failure which is not because of our experiment then we should abort our experiment. This is to ensure that we don't make the troubleshooting of the issue more ambiguous or complex due to multiple workflows triggered as part of our experiments. If our validation of hypothesis ends up with actual failure than we should be capturing all the required metrics and information that can help us analyze the vulnerabilities of our system. This will help us define some actionable items to harden different aspects of our system. If our validation of hypothesis ends up with success, we should be scaling up (i.e. we either change the blast radius or increase it) and repeat the validations.
Closing Thoughts

We tried understanding the WHAT'S, WHY'S and HOW's of Chaos Engineering with required rationale. So now we will be pretty much convinced that failures in software world are inevitable. And hence we need a mechanism to make our system better prepared to deal with them - That mechanism is none other than Chaos Engineering which will help us to identify potential weaknesses and vulnerability and thereby help us to harden them. This will be of immense help to be better prepared for production incidents and outages by either proactively getting rid of them or by reducing Mean Time To Recover (MTTR) from those unwanted incidents and outages.

In second part I will be demonstrating Chaos Engineering with a working example by following what we have learnt over here. So stay tuned!
###################################################################################################################################################

Spring Boot helps developers to implement enterprise grade applications which can be pushed to production in no time. Once application gets into production and if we strongly believe in vedic philosophy of Karma :), we are bound to experience Murphy's Law.

    Whatever can go wrong, will go wrong

Considering nature of Distributed Architecture, Observability and Monitoring of application and infrastructure is of paramount importance. Thanks to the Spring Boot team, who has shipped Spring Boot Actuator from the very first release.

In order to understand Spring Boot Actuator and its usage w.r.t observability and monitoring in real world application, lets construct a system with hypothetical requirements - We will implement an application which consists of two applications -

    Reservation Endpoints - which consists of public APIs for managing Reservation workflows
    Reservation Service - responsible for catering to Command operations (as a part of CQS). It is primarily entrusted with responsibility of consuming events / messages pertaining to Save Reservation operation

Application Architecture

Sample App Architecture

Sample App Architecture

Source code of both the applications can be referred at -

    Reservation Endpoint
    Reservation Service

Below are the APIs exposed by Reservation Endpoints

| Business Operation | URI | HTTP Method | Description | | -------- | -------- | ------ | | Add Reservation | /reservation | POST | Allows to add Reservation with user details | | Fetch all Reservations | /reservations | GET | Returns list of all the users whose reservation has been done | | Fetch all Reservations by first name | /reservation/firstName/{firstName} | GET | Returns list of all the users with given first name whose reservation has been done | | Fetch all Reservations by last name | /reservation/lastName/{lastName} | GET | Returns list of all the users with given last name whose reservation has been done |

Before we delve into nuances of Monitoring and Observability of distributed systems, lets understand how to get required metrics

Spring Boot Team has shipped Spring Boot Actuator from the very first release.

So Spring Boot actuator exposes set of endpoints over HTTP or JMX. It can be enabled by adding 'spring-boot-starter-actuator' dependency within build file -

Maven Configurations

1<dependencies>
2	<dependency>
3		<groupId>org.springframework.boot</groupId>
4		<artifactId>spring-boot-starter-actuator</artifactId>
5	</dependency>
6</dependencies>



	
		

	


	
		

	


	
		

	


	
		

	

































	
		

	


	
		

	


	
		

	


	
		

	


	
		

	


	
		

	































java

Gradle Configurations

1dependencies {
2	compile("org.springframework.boot:spring-boot-starter-actuator")
3}



	
		

	


	
		

	


	
		

	


	
		

	

































	
		

	


	
		

	


	
		

	


	
		

	


	
		

	


	
		

	































java

When you run your application, it exposes set of endpoints relative to '/actuator' URI which are as shown below

The same can be viewed over JMX

One can see the complete list of actuator endpoints at the official documentation.

Spring Boot properties that can be used for enabling / disabling actuator endpoints -

1########################## Actuator Configurations
2# Use "\*" to expose all endpoints, or a comma-separated list to expose selected ones i.e. health,info
3management.endpoints.web.exposure.include = \*
4
5# Use "\*" to expose all endpoints, or a comma-separated list to expose selected ones
6management.endpoints.jmx.exposure.include = \*



	
		

	


	
		

	


	
		

	


	
		

	

































	
		

	


	
		

	


	
		

	


	
		

	


	
		

	


	
		

	































java
Customization of Management Endpoints

Since its inception, Spring has always preferred giving customization and extension hook points at appropriate places within its framework. Same holds good for Actuator endpoints - which is apparent from below table
Customiation Type	Key within application.properties	Example	Description
Customizing management endpoint	management.endpoints.web.base-path	management.endpoints.web.base-path=/abc	Changes the endpoint from /actuator/{id} to /abc/{id}
Customizing management server address	management.server.address	management.server.address=127.1.2.3	Customizes management server's address to 127.1.2.3. This may be required if one wants to listen only on ops monitored network
Customizing management server port	management.server.port	management.server.port=8181	Changes default port of management server to 8181. This may be needed if one has specific need of exposing endpoint on separate HTTP port (governed by the rules of its data centre)
Disable HTTP endpoints	management.server.port OR management.endpoints.web.exposure.exclude	management.server.port=-1 OR management.endpoints.web.exposure.exclude=/*	Disables all the HTTP endpoints exposed as a part of Actuator framework

Note - Management endpoints can be secured in terms of its access by configuring required ssl configuration within application.properties file
Health Information

In today's contemporary world of distributed architecture, gathering health information about application and infrastructure is imperative - as it not only helps to monitor the system but also helps to setup alerts for proactive actions that need to be taken. With actuator one can get detailed health information about application along with its infrastructure by configuring below property within application.properties file

1########################## Actuator Configurations
2
3# To get the complete details including the status of every health indicator that was checked as 
4# part of the health check-up process
5management.endpoint.health.show-details=always



	
		

	


	
		

	


	
		

	


	
		

	

































	
		

	


	
		

	


	
		

	


	
		

	


	
		

	


	
		

	































java

By enabling health endpoints within our application, we will not only be able to see application's health information but we will also get to see infrastructure's health i.e. MongoDB and Rabbitmq as they are auto-configured

Metrics

Spring Boot mainly provides below metrics via Actuator

    JVM metrics, report utilization of:
        Various memory and buffer pools
        Statistics related to garbage collection
        Threads utilization
        Number of classes loaded/unloaded
    CPU metrics
    File descriptor metrics
    Logback metrics: record the number of events logged to Logback at each log level
    Uptime metrics: report a gauge for uptime and a fixed gauge representing the application’s absolute start time
    Tomcat metrics
    Spring Integration metrics
    Spring MVC metrics
    Spring Webflux metrics
    RestTemplate metrics

So with this understanding of basic capabilities of Spring Boot's Actuator, can we perform enterprise monitoring? Tentatively YES, but lot of things need to be built from scratch viz. Application that constantly fetches data from Actuator and then some how renders the captured data in human readable form. Spring framework community once again absolves software fraternity from doing such heavy lifting - Actuator provides dependency management and auto configuration of Micrometer. Micrometer is

"vendor-neutral interfaces for timers, gauges, counters, distribution summaries, and long task timers with a dimensional data model that, when paired with a dimensional monitoring system, allows for efficient access to a particular named metric with the ability to drill down across its dimensions"

And Micrometer in principle -

    is metrics supporting library for JVM application
    supports plethora of monitoring tools viz. Atlas, Datadog, Graphite, Prometheus etc.

We will be using Prometheus to demonstrate its capabilities using above example. In order to configure Prometheus, we just need to add below gradle dependency

1dependencies {
2	compile('io.micrometer:micrometer-registry-prometheus')
3}



	
		

	


	
		

	


	
		

	


	
		

	

































	
		

	


	
		

	


	
		

	


	
		

	


	
		

	


	
		

	































java

When we access /actuator/prometheus endpoint we can see lot of information pertaining to -

    JVM Memory
    Garbage Collection
    Disk Space
    Rabbitmq
    Spring Integration channels

so on and so forth. Looking at the screen shot below one can realize that it is certainly not that user friendly in terms of inferring from data that is being emitted

So we will be using Prometheus and Grafana to easily infer from the data emitted as a part of Spring Boot Actuator. Prometheus is an open source monitoring tool developed by SoundCloud.  Grafana is an open platform for beautiful monitoring and analytics of time series data.
Setting up Prometheus

Download Prometheus for your corresponding environment. Update _prometheus.yml _within its root as per below configurations

 1global:
 2  scrape\_interval:     15s # By default, scrape targets every 15 seconds.
 3scrape\_configs:
 4  - job\_name: 'prometheusJN'
 5    scrape\_interval: 5s
 6    static\_configs:
 7      - targets: \['localhost:9090'\]
 8  - job\_name: 'springJN1'
 9    scrape\_interval: 5s
10    metrics\_path: '/actuator/prometheus'
11    static\_configs:
12      - targets: \['localhost:8090'\]
13  - job\_name: 'springJN2'
14    scrape\_interval: 5s
15    metrics\_path: '/actuator/prometheus'
16    static\_configs:
17      - targets: \['localhost:8091'\]

...


	
		

	


	
		

	


	
		

	


	
		

	

































	
		

	


	
		

	


	
		

	


	
		

	


	
		

	


	
		

	































java

Note - 8090 and 8091 are the 2 ports on which my applications i.e. Reservation Endpoint and Reservation Service are running respectively

Run Prometheus by executing following command

1_.\\prometheus.exe --config.file=prometheus.yml_.



	
		

	


	
		

	


	
		

	


	
		

	

































	
		

	


	
		

	


	
		

	


	
		

	


	
		

	


	
		

	































java

Once Prometheus is successfully up and running you can see below console which means that it is not only running on 9090 but also scrapping metrics for you as per above configurations

Setting up Grafana

Download Grafana for your corresponding environment. Setting up Grafana will be a 3 step process :
1. Datasource Configuration

Update grafana-datasource.yml (at <GRAFANA_HOME>/conf/provisioning/datasources) file with below configurations -

1apiVersion: 1
2
3datasources:
4- name: prometheusDS
5  type: prometheus
6  access: direct
7  url: http://localhost:9090



	
		

	


	
		

	


	
		

	


	
		

	

































	
		

	


	
		

	


	
		

	


	
		

	


	
		

	


	
		

	































java
2. Dashboard Configuration

Update grafana-dashboard.yml (at <GRAFANA_HOME>/conf/provisioning/dashboards) file with below configurations

1apiVersion: 1
2
3providers:
4- name: 'default'
5  folder: ''
6  type: file
7  options:
8    path: H:\\\\softwares\\\\Tech\\\\grafana-5.1.3\\\\public\\\\dashboards



	
		

	


	
		

	


	
		

	


	
		

	

































	
		

	


	
		

	


	
		

	


	
		

	


	
		

	


	
		

	































java
3. Adding Dashboard

Copy mygrafana-dashboard.json file within <GRAFANA_HOME>/public/dashboards

Start Grafana server by executing following command

> .\\grafana-server.exe

Note -  All the above configuration files of Prometheus and Grafana are available here

Login to Grafana console with username and password. Configure Prometheus data source so that it can get data from Prometheus to render it to the UI. Along with data source, we will add some pre-configured dashboards to visualize health of our applications viz.

    Java Micrometer Dashboard
    Spring Boot Statistics Dasboard
    Spring Throughput Dashboard

So with this entire setup we are equipped with necessary infrastructure to perform enterprise monitoring of our application.

We will now simulate load comprising of all 3 API calls -

    Save Reservation
    Fetch all Reservations
    Fetch specific Reservation

Note - We can even simulate load using Apache Bench which provides finer control in terms of performance testing and benchmarking

Below are some of the screen shots taken from Grafana dashboards, which are giving some insights about the state of application when it was under load
Basic Statistics and CPU usage

Logback Statistics

JVM Statistics

Application Throughput

Conclusion

So we saw how Spring Boot provides exhaustive time series metrics about different facets of health of an application which can help team to infer lot of insights from the available data by using monitoring and rendering tools like Prometheus / Grafana. Having said that, usage of Grafana is not just about setting up and configuring dashboards - one can create his own dashboard based on type of insights one would really like to have. One should also consider infrastructure (MongoDB and Rabbitmq here) as key aspect of the application and hence they should also be monitored closely via monitoring tools - this might be a candidate of another blog sometime later.

