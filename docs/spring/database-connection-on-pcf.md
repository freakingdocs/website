---
title: "Database Connection on PCF"
date: 2018-09-17T13:45:49-07:00
anchor: "database-connection-on-pcf"
weight: 10
---

At some point in your life you'll want to connect to a database on PCF. That's cool. 
Once you provision a service, it'll jam a bunch of information in a JSON string that shows up as an environment variable called `VCAP_SERVICES`.
It's kind of a bummer to parse JSON in an environment variable, but fear not! Spring Boot does this for you!

All you need to do is reference the values you want under `vcap.services.<SERVICE-INSTANCE>` where `<SERVICE-INSTANCE>` is the 
name you specified when you created the service on PCF. 

So your spring application properties file for your database might look like this...
```
spring.datasource.url=${vcap.services.<MY-COOL-DATABASE-SERVICE-NAME>.credentials.jdbc_uri}
spring.datasource.username=${vcap.services.<MY-COOL-DATABASE-SERVICE-NAME>.credentials.username}
spring.datasource.password=${vcap.services.<MY-COOL-DATABASE-SERVICE-NAME>.credentials.password}
```

If you don't want to use "vcap", and would rather have words like "cloud" you can use Springs Cloud Connectors. [Both methods are detailed here.](https://spring.io/blog/2015/04/27/binding-to-data-services-with-spring-boot-in-cloud-foundry)
