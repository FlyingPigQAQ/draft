- 文档1  

```yaml
#############################################
######    Spring2go Zuul Configuration     ######
#############################################

zuul.debug.request=true
zuul.debug.parameter=debugRequest
#zuul.include.debug.header=true
zuul.filter.dao.type=jdbc
archaius.deployment.applicationId=mobile_zuul
#zuul.filter.repository=http://192.168.100.106:80/filters
zuul.filter.pre.path=D:/temp/scripts/pre
zuul.filter.route.path=D:/temp/scripts/route
zuul.filter.post.path=D:/temp/scripts/post
zuul.filter.error.path=D:/temp/scripts/error
#############################################


#############################################
######    Filters Dao Source          ######
#############################################
zuul.filter.poller.enabled=true
zuul.filter.poller.interval=30000
zuul.filter.table.name=zuul_filter
zuul.data-source.class-name=com.mysql.jdbc.jdbc2.optional.MysqlDataSource
zuul.data-source.url=jdbc:mysql://localhost:3306/spring2go_zuul_filter
zuul.data-source.user=root
zuul.data-source.password=mysql
zuul.data-source.min-pool-size=10
zuul.data-source.max-pool-size=20
zuul.data-source.connection-timeout=1000
zuul.data-source.idle-timeout=600000
zuul.data-source.max-lifetime=1800000
#############################################


#############################################
######    Eureka Configuration         ######
#############################################
eureka.enabled=false
eureka.region=default
eureka.name=MobileZuul
#should be the same as web server port
eureka.port=1113
eureka.vipAddress=mobile_zuul.spring2go.com
eureka.preferSameZone=false
eureka.shouldUseDns=false
eureka.serviceUrl.default=http://192.168.100.120:1113/eureka/,http://192.168.100.121:1113/eureka/,http://192.168.100.122:1113/eureka/
eureka.default.availabilityZones=default
eureka.asgName=MobileZuul
#############################################


#############################################
######    Hystrix                      ######
#############################################
hystrix.command.default.execution.isolation.semaphore.maxConcurrentRequests=100
hystrix.threadpool.default.coreSize=10
hystrix.command.default.execution.isolation.thread.timeoutInMilliseconds=1500
hystrix.command.default.circuitBreaker.enabled=true
hystrix.command.default.circuitBreaker.forceOpen=false
hystrix.command.default.circuitBreaker.forceClosed=false
hystrix.command.default.circuitBreaker.requestVolumeThreshold=10
hystrix.command.default.circuitBreaker.errorThresholdPercentage=30
hystrix.command.default.circuitBreaker.sleepWindowInMilliseconds=10000
#############################################

#############################################
######    Custom Route                      ######
#############################################

# TO BE ADDED
zuul.routing_table_string=student@http://localhost:9700&hello@http://localhost:9700
```
- 文档二
```yaml

```
