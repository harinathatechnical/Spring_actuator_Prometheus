# Spring_actuator_Prometheus

Spring Boot Actuator module helps you monitor and manage your Spring Boot application by providing production-ready features like health check-up, auditing, metrics gathering, HTTP tracing etc. All of these features can be accessed over JMX or HTTP endpoints.

Actuator also integrates with external application monitoring systems like Prometheus, Graphite, DataDog, Influx, Wavefront, New Relic and many more. These systems provide you with excellent dashboards, graphs, analytics, and alarms to help you monitor and manage your application from one unified interface.

Actuator uses Micrometer, an application metrics facade to integrate with these external application monitoring systems. This makes it super easy to plug-in any application monitoring system with very little configuration.

1. Create Spring boot project :-
      
            a) create a maven project 
            b) open the pom and add the followind details 
      
            <parent>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-starter-parent</artifactId>
              <version>2.1.3.RELEASE</version>
              <relativePath></relativePath>
          </parent>

          <dependencies>
              <dependency>
                  <groupId>org.springframework.boot</groupId>
                  <artifactId>spring-boot-starter-web</artifactId>
              </dependency>
          </dependencies>

          <build>
              <plugins>
                  <plugin>
                      <groupId>org.springframework.boot</groupId>
                      <artifactId>spring-boot-maven-plugin</artifactId>
                  </plugin>
              </plugins>
          </build>
          
  add the actuator Dependency in pom 
        
         <dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-actuator</artifactId>
	  </dependency>
    
    
   Add the main class 
   
       import org.springframework.boot.SpringApplication;

            @org.springframework.boot.autoconfigure.SpringBootApplication
            public class SpringBootApplicationMonitor {
                   public static void main(String[] args) {
                        SpringApplication.run(SpringBootApplicationMonitor.class,args);
                  }

            }
            
 Change the PORT to 8090 using application.properties 
 
 Start the application and test http://localhost:8090/actuator will give the below reaults
 
      {
            _links: {
            self: {
            href: "http://localhost:8090/actuator",
            templated: false
            },
            health: {
            href: "http://localhost:8090/actuator/health",
            templated: false
            },
            health-component-instance: {
            href: "http://localhost:8090/actuator/health/{component}/{instance}",
            templated: true
            },
            health-component: {
            href: "http://localhost:8090/actuator/health/{component}",
            templated: true
            },
            info: {
            href: "http://localhost:8090/actuator/info",
            templated: false
            }
            }
        }
 
Actuator creates several so-called endpoints that can be exposed over HTTP or JMX to let you monitor and interact with your application.

For example, There is a /health endpoint that provides basic information about the application’s health. The /metrics endpoint shows several useful metrics information like JVM memory used, system CPU usage, open files, and much more. The /loggers endpoint shows application’s logs and also lets you change the log level at runtime.

Note that, every actuator endpoint can be explicitly enabled and disabled. Moreover, the endpoints also need to be exposed over HTTP or JMX to make them remotely accessible.

Let’s first run the application and try to access the actuator endpoints that are enabled and exposed over HTTP by default. After that, we’ll learn how to enable more actuator endpoints and also expose them over HTTP.

Let’s explore the health endpoint by opening the http://localhost:8090/actuator/health URL. The endpoint should display the following -

            {
                "status": "UP"
            }
            
  The status will be UP as long as the application is healthy. It will show DOWN if the application gets unhealthy due to any issue like connectivity with the database or lack of disk space etc. Check out the next section to learn more about how Spring Boot determines the health of your application and how you can tweak it.
  
  The info endpoint (http://localhost:8090/actuator/info) displays general information about your application obtained from build files like META-INF/build-info.properties or Git files like git.properties or through any environment property under the key info. You’ll learn how to tweak the output of this endpoint in the next section.

By default, only the health and info endpoints are exposed over HTTP. That’s why the /actuator page lists only the health and info endpoints. We’ll learn how to expose other endpoints shortly. First, Let’s see what those endpoints are and what do they offer you.

            Endpoint ID	Description
            auditevents	Exposes audit events (e.g. auth_success, order_failed) for your application
            info	Displays information about your application.
            health	Displays your application’s health status.
            metrics	Shows various metrics information of your application.
            loggers	Displays and modifies the configured loggers.
            logfile	Returns the contents of the log file (if logging.file or logging.path properties are set.)
            httptrace	Displays HTTP trace info for the last 100 HTTP request/response.
            env	Displays current environment properties.
            flyway	Shows details of Flyway database migrations.
            liquidbase	Shows details of Liquibase database migrations.
            shutdown	Lets you shut down the application gracefully.
            mappings	Displays a list of all @RequestMapping paths.
            scheduledtasks	Displays the scheduled tasks in your application.
            threaddump	Performs a thread dump.
            heapdump	Returns a GZip compressed JVM heap dump.
            
            
Enabling and Disabling Actuator Endpoints

By default, all the endpoints that I listed in the previous section are enabled except the shutdown endpoint.

You can enable or disable an actuator endpoint by setting the property management.endpoint.<id>.enabled to true or false (where id is the identifier for the endpoint).

For example, to enable the shutdown endpoint, add the following to your application.properties file -

             management.endpoint.shutdown.enabled=true
             
Exposing Actuator Endpoints
By default, all the actuator endpoints are exposed over JMX but only the health and info endpoints are exposed over HTTP.

Here is how you can expose actuator endpoints over HTTP and JMX using application properties -             
             
Exposing Actuator Endpoints
By default, all the actuator endpoints are exposed over JMX but only the health and info endpoints are exposed over HTTP.

Here is how you can expose actuator endpoints over HTTP and JMX using application properties -

            # Use "*" to expose all endpoints, or a comma-separated list to expose selected ones
            management.endpoints.web.exposure.include=health,info 
            management.endpoints.web.exposure.exclude=

Exposing Actuator endpoints over JMX

         # Use "*" to expose all endpoints, or a comma-separated list to expose selected ones
         management.endpoints.jmx.exposure.include=*
         management.endpoints.jmx.exposure.exclude=
         
Let’s expose all actuator endpoints by setting the property management.endpoints.web.exposure.include to * and check the output of http://localhost:8090/actuator page. You’ll notice that the actuator page now lists all the enabled endpoints-     

         {
            _links: {
            self: {
            href: "http://localhost:8090/actuator",
            templated: false
            },
            auditevents: {
            href: "http://localhost:8090/actuator/auditevents",
            templated: false
            },
            beans: {
            href: "http://localhost:8090/actuator/beans",
            templated: false
            },
            caches-cache: {
            href: "http://localhost:8090/actuator/caches/{cache}",
            templated: true
            },
            caches: {
            href: "http://localhost:8090/actuator/caches",
            templated: false
            },
            health-component: {
            href: "http://localhost:8090/actuator/health/{component}",
            templated: true
            },
            health-component-instance: {
            href: "http://localhost:8090/actuator/health/{component}/{instance}",
            templated: true
            },
            health: {
            href: "http://localhost:8090/actuator/health",
            templated: false
            },
            conditions: {
            href: "http://localhost:8090/actuator/conditions",
            templated: false
            },
            shutdown: {
            href: "http://localhost:8090/actuator/shutdown",
            templated: false
            },
            configprops: {
            href: "http://localhost:8090/actuator/configprops",
            templated: false
            },
            env: {
            href: "http://localhost:8090/actuator/env",
            templated: false
            },
            env-toMatch: {
            href: "http://localhost:8090/actuator/env/{toMatch}",
            templated: true
            },
            info: {
            href: "http://localhost:8090/actuator/info",
            templated: false
            },
            loggers: {
            href: "http://localhost:8090/actuator/loggers",
            templated: false
            },
            loggers-name: {
            href: "http://localhost:8090/actuator/loggers/{name}",
            templated: true
            },
            heapdump: {
            href: "http://localhost:8090/actuator/heapdump",
            templated: false
            },
            threaddump: {
            href: "http://localhost:8090/actuator/threaddump",
            templated: false
            },
            metrics-requiredMetricName: {
            href: "http://localhost:8090/actuator/metrics/{requiredMetricName}",
            templated: true
            },
            metrics: {
            href: "http://localhost:8090/actuator/metrics",
            templated: false
            },
            scheduledtasks: {
            href: "http://localhost:8090/actuator/scheduledtasks",
            templated: false
            },
            httptrace: {
            href: "http://localhost:8090/actuator/httptrace",
            templated: false
            },
            mappings: {
            href: "http://localhost:8090/actuator/mappings",
            templated: false
            }
            }
            }
            
/health endpoint
   The health endpoint checks the health of your application by combining several health indicators.

Spring Boot Actuator comes with several predefined health indicators like DataSourceHealthIndicator, DiskSpaceHealthIndicator, MongoHealthIndicator, RedisHealthIndicator, CassandraHealthIndicator etc. It uses these health indicators as part of the health check-up process.

For example, If your application uses Redis, the RedisHealthIndicator will be used as part of the health check-up. If your application uses MongoDB, the MongoHealthIndicator will be used as part of the health check-up and so on.

You can also disable a particular health indicator using application properties like so -

management.health.mongo.enabled=false
But by default, all these health indicators are enabled and used as part of the health check-up process.

Displaying detailed health information
The health endpoint only shows a simple UP or DOWN status. To get the complete details including the status of every health indicator that was checked as part of the health check-up process, add the following property in the application.properties file -

            # HEALTH ENDPOINT
            management.endpoint.health.show-details=always
Once you do that, the health endpoint will display more details like this -

            {
               "status":"UP",
               "details":{
                  "diskSpace":{
                     "status":"UP",
                     "details":{
                        "total":250790436864,
                        "free":100327518208,
                        "threshold":10485760
                     }
                  }
               }
            }
The health endpoint now includes the details of the DiskSpaceHealthIndicator which is run as part of the health checkup process.

/metrics endpoint
The /metrics endpoint lists all the metrics that are available for you to track.

       {
            names: [
            "jvm.memory.max",
            "jvm.threads.states",
            "jvm.gc.memory.promoted",
            "jvm.memory.used",
            "jvm.gc.max.data.size",
            "jvm.gc.pause",
            "jvm.memory.committed",
            "system.cpu.count",
            "logback.events",
            "http.server.requests",
            "tomcat.global.sent",
            "jvm.buffer.memory.used",
            "tomcat.sessions.created",
            "jvm.threads.daemon",
            "system.cpu.usage",
            "jvm.gc.memory.allocated",
            "tomcat.global.request.max",
            "tomcat.global.request",
            "tomcat.sessions.expired",
            "jvm.threads.live",
            "jvm.threads.peak",
            "tomcat.global.received",
            "process.uptime",
            "tomcat.sessions.rejected",
            "process.cpu.usage",
            "tomcat.threads.config.max",
            "jvm.classes.loaded",
            "jvm.classes.unloaded",
            "tomcat.global.error",
            "tomcat.sessions.active.current",
            "tomcat.sessions.alive.max",
            "jvm.gc.live.data.size",
            "tomcat.threads.current",
            "jvm.buffer.count",
            "jvm.buffer.total.capacity",
            "tomcat.sessions.active.max",
            "tomcat.threads.busy",
            "process.start.time"
            ]
         }
         
   To get the details of an individual metric, you need to pass the metric name in the URL like this -

      http://localhost:8090/actuator/metrics/{MetricName}.

For example, to get the details of system.cpu.usage metric, use the URL http://localhost:8090/actuator/metrics/system.cpu.usage. This will display the details in JSON format like so -

            {
               "name":"system.cpu.usage",
               "measurements":[
                  {
                     "statistic":"VALUE",
                     "value":0.11774479321066383
                  }
               ],
               "availableTags":[

               ]
            }      
            
            
 /loggers endpoint
The loggers endpoint, which can be accessed at http://localhost:8090/actuator/loggers, displays a list of all the configured loggers in your application with their corresponding log levels.

You can also view the details of an individual logger by passing the logger name in the URL like this -

      http://localhost:8090/actuator/loggers/{name}.

For example, to get the details of the root logger, use the URL http://localhost:8090/actuator/loggers/root.

            {
               "configuredLevel":"INFO",
               "effectiveLevel":"INFO"
            }
Changing log levels at runtime.
The loggers endpoint also allows you to change the log level of a given logger in your application at runtime.

For example, To change the log level of the root logger to DEBUG at runtime, make a POST request to the URL http://localhost:8090/actuator/loggers/root with the following payload -

            {
               "configuredLevel": "DEBUG"
            }
This functionality will really be useful in cases when your application is facing issues in production and you want to enable DEBUG logging for some time to get more details about the issue.

To reset the log-level to the default value, you can pass a value of null in the configuredLevel field.

/info endpoint
The info endpoint displays arbitrary information about your application. It obtains build information from META-INF/build-info.properties file, Git information from git.properties file. It also displays any information available in environment properties under the key info.

You can add properties under the key info in application.properties file like so -

            # INFO ENDPOINT CONFIGURATION
            info.app.name=@project.name@
            info.app.description=@project.description@
            info.app.version=@project.version@
            info.app.encoding=@project.build.sourceEncoding@
            info.app.java.version=@java.version@
Note that, I’m using Spring Boot’s Automatic property expansion feature to expand properties from the maven project.

Once you add the above properties, the info endpoint (http://localhost:8090/actuator/info) will start displaying the information like so -

            {
               "app":{
                  "name":"actuator-demo",
                  "description":"Spring Boot Actuator Demo Project",
                  "version":"0.0.1-SNAPSHOT",
                  "encoding":"UTF-8",
                  "java":{
                     "version":"1.8.0_112"
                  }
               }
            }           
            
            
            
            
Prometheus

Prometheus is an open-source monitoring system that was originally built by SoundCloud. It consists of the following core components -

A data scraper that pulls metrics data over HTTP periodically at a configured interval.

A time-series database to store all the metrics data.

A simple user interface where you can visualize, query, and monitor all the metrics.

Grafana

Grafana allows you to bring data from various data sources like Elasticsearch, Prometheus, Graphite, InfluxDB etc, and visualize them with beautiful graphs.

It also lets you set alert rules based on your metrics data. When an alert changes state, it can notify you over email, slack, or various other channels.


Spring Boot uses Micrometer, an application metrics facade to integrate actuator metrics with external monitoring systems.

It supports several monitoring systems like Netflix Atlas, AWS Cloudwatch, Datadog, InfluxData, SignalFx, Graphite, Wavefront, Prometheus etc.

To integrate actuator with Prometheus, you need to add the micrometer-registry-prometheus dependency -
    
      <!-- Micrometer Prometheus registry  -->
	<dependency>
		<groupId>io.micrometer</groupId>
		<artifactId>micrometer-registry-prometheus</artifactId>
	</dependency>
	
Once you add the above dependency, Spring Boot will automatically configure a PrometheusMeterRegistry and a CollectorRegistry to collect and export metrics data in a format that can be scraped by a Prometheus server.

All the application metrics data are made available at an actuator endpoint called /prometheus. The Prometheus server can scrape this endpoint to get metrics data periodically.

Let’s explore the prometheus endpoint that is exposed by Spring Boot when micrometer-registry-prometheus dependency is available on the classpath.

First of all, you’ll start seeing the prometheus endpoint on the actuator endpoint-discovery page (http://localhost:8090/actuator) -

The prometheus endpoint exposes metrics data in a format that can be scraped by a Prometheus server. You can see the exposed metrics data by navigating to the prometheus endpoint (http://localhost:8090/actuator/prometheus) -


	# HELP jvm_buffer_memory_used_bytes An estimate of the memory that the Java virtual machine is using for this buffer pool
		# TYPE jvm_buffer_memory_used_bytes gauge
		jvm_buffer_memory_used_bytes{id="direct",} 81920.0
		jvm_buffer_memory_used_bytes{id="mapped",} 0.0
		# HELP jvm_threads_live The current number of live threads including both daemon and non-daemon threads
		# TYPE jvm_threads_live gauge
		jvm_threads_live 23.0
		# HELP tomcat_global_received_bytes_total  
		# TYPE tomcat_global_received_bytes_total counter
		tomcat_global_received_bytes_total{name="http-nio-8080",} 0.0
		# HELP jvm_gc_pause_seconds Time spent in GC pause
		# TYPE jvm_gc_pause_seconds summary
		jvm_gc_pause_seconds_count{action="end of minor GC",cause="Allocation Failure",} 7.0
		jvm_gc_pause_seconds_sum{action="end of minor GC",cause="Allocation Failure",} 0.232
		jvm_gc_pause_seconds_count{action="end of minor GC",cause="Metadata GC Threshold",} 1.0
		jvm_gc_pause_seconds_sum{action="end of minor GC",cause="Metadata GC Threshold",} 0.01
		jvm_gc_pause_seconds_count{action="end of major GC",cause="Metadata GC Threshold",} 1.0
		jvm_gc_pause_seconds_sum{action="end of major GC",cause="Metadata GC Threshold",} 0.302
		# HELP jvm_gc_pause_seconds_max Time spent in GC pause
		# TYPE jvm_gc_pause_seconds_max gauge
		jvm_gc_pause_seconds_max{action="end of minor GC",cause="Allocation Failure",} 0.0
		jvm_gc_pause_seconds_max{action="end of minor GC",cause="Metadata GC Threshold",} 0.0
		jvm_gc_pause_seconds_max{action="end of major GC",cause="Metadata GC Threshold",} 0.0
		# HELP jvm_gc_live_data_size_bytes Size of old generation memory pool after a full GC
		# TYPE jvm_gc_live_data_size_bytes gauge
		jvm_gc_live_data_size_bytes 5.0657472E7

		## More data ......
		
Add the below files into Promethues.yml file We need to configure Prometheus to scrape metrics data from Spring Boot Actuator’s /prometheus endpoint.
	
		job_name: 'spring-actuator'
		    metrics_path: '/actuator/prometheus'
		    scrape_interval: 5s
		    static_configs:
		    - targets: ['HOST_IP:8090']

The above configuration file is an extension of the basic configuration file available in the Prometheus documentation.

The most important stuff to note in the above configuration file is the spring-actuator job inside scrape_configs section.

The metrics_path is the path of the Actuator’s prometheus endpoint. The targets section contains the HOST and PORT of your Spring Boot application.

Please make sure to replace the HOST_IP with the IP address of the machine where your Spring Boot application is running. Note that, localhost won’t work here because we’ll be connecting to the HOST machine from the docker container. You must specify the network IP address.

That’s it! You can now navigate to http://localhost:9090 to explore the Prometheus dashboard.

You can enter a Prometheus query expression inside the Expression text field and visualize all the metrics for that query.

Following are some Prometheus graphs for our Spring Boot application’s metrics -

Graphana install and browse using http://localhost:3000/?orgId=1 default admin / admin .

Follow the steps below to import metrics from Prometheus and visualize them on Grafana:

1. Add the Prometheus data source in Grafana

2. Create a new Dashboard with a Graph
3. Add a Prometheus Query expression in Grafana’s query editor
4. Visualize metrics from Grafana’s dashboard



		
