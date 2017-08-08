This project is to demonstrate the use of a Configuration server.

Application Configuration Server is a dedicated, dynamic and centralized key/value store which may be a distributed or non distributed server. It provides things like auditing, versioning and also has support for Cryptography. Spring Cloud provides quite a few options for configuration management. A few of such options correspond to as an association with various third-party tools such as Consul and Zookeeper. The other option is the Spring Cloud Configuration server which is developed by the Spring cloud team. Spring Cloud configuration also provides a Spring Cloud Config Client. The Config Client is usually embedded within the application service through Spring Environment abstraction.

At the core a config server is a web application that provides a REST based access to the configuration files stored in the backend such as in a File system or SVN or Git(default). It supports different output formats such as JSON, Properties and YAML. Additionally, it also supports various configuration scopes such as "application specific({application}.properties)", "global(eg. application.yml or application.properties)" that applies to all applications and "spring profile specific(eg. {application}-{profile})".

The annotation "@EnableConfigServer" in the SpringBoot application main class turns it as the config server. In order to make it discoverable via service discovery then add the eureka client dependencies, configure the service discovery url and add the annotation "@EnableDiscoveryClient" to this class.

Each of the available REST endpoints in a config server share a common set of parameters. The values of these parameters influence the configuration which is returned. The first parameter is the {application} parameter, which maps to the "spring.application.name" property as configured in the client. The next parameter is the spring {profile} parameter. It refers to the value of the property "spring.profiles.active". The last parameter is the {label} which is a server side feature for grouping your configuration files into an arbitrary named sets that corresponds to the backend type that you are using to store the configuration files. For example, if you are using Git as the backend store, then the {label} translates to the Git branch.

The REST endpoints are invoked as: 
1) GET /{application}/{profile}/[/{label}]
Example: /myapp/dev/master
		/myapp/default (When you do not set an active profile)

2) GET /{application}-{dev}.(yml | properties)
Example: /myapp-dev.yml
		/myapp-prod.properties
	   /myapp-default.properties

3) GET /{label}/{application}-{profile}.(yml | properties)
Example: /master/myapp-dev.yml
         /master/myapp-default.properties
         
In order to quickly bootstrap it, go to http://start.spring.io/ and use the Spring Initializr to stub it out.
Generate it with the dependencies Eureka Discovery(optional), Config Server and the Actuator.         	   		 
application properties:

#Server port
sever.port=8888

#Location of the Git repository that the configuration server is going to serve.
spring.cloud.config.server.git.uri=https://github.com/anantac1977/spring-cloud-config-server-properties.git

#application service name
spring.application.name=configserver

#Discovery server address
eureka.client.service-url.defaultZone=http://localhost:8761/eureka
