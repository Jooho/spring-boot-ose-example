# Use application.properties to get string.

This tutorial change the spring application to get string from application.properties file.


## Update source

src/main/java/hello/helloController.java
```
package hello;

import org.springframework.beans.factory.annotation.Value;           <====== ADD
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.bind.annotation.RequestMapping;

@RestController
public class HelloController {


   @Value("${hello.message}")    <==== ADD
   private String message;       <==== ADD

    @RequestMapping("/")
    public String index() {
        return "Greetings from Spring Boot! - It is from " + message;       <== Changed
    }


```

## ADD application.properties

```
$ mkdir src/main/resources
$ echo "hello.message=Local" > src/main/resources/application.properties
```

## Build and Deploy
```
$ mvn package && java -jar target/gs-spring-boot-0.1.0.jar
 ....
.   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::        (v1.2.2.RELEASE)

....
```

## Test with curl command
```
$ curl localhost:8080

Greetings from Spring Boot! - It is from Local
```


Now, you have spring boot application that use application.properies !.

Move to next tutorial.

