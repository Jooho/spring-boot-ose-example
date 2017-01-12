# Deploy Spring boot on OpenShift Container Platform

This tutorial deploy your application with S2I-java builder image.

## Create S2I image for spring boot ( There is no official s2i image yet for deploying executable jar - until OCP 3.3 )


```
$ ls 
spring-boot-test  

$ git clone https://github.com/Jooho/spring-boot-ose-example

$ cd spring-boot-ose-example/ose_scripts/s2i

$ oc login 

$ oc new-project spring-boot-s2i-demo

$ oc create -f java-s2i-build-image-template.json

$ oc get is
NAME       DOCKER REPO                                         TAGS                         UPDATED
rhel7      registry.access.redhat.com/rhel7/rhel               7.3,7.2,7.0-21 + 2 more...   19 seconds ago
s2i-java   172.30.100.175:5000/spring-boot-s2i-demo/s2i-java   latest                       15 seconds ago
```

## Create s2i-java template
```
$ cd ../spring-cloud-kubernetes-sample

$ oc create -f spring-boot-s2i-template.json

```

## Create new app using s2i-java template 
```
$ oc new-app --template=s2i-java -p APPLICATION_SOURCE_REPOSITORY_URL=https://github.com/${YOUR_GITHUB}/spring-cloud-kubernetes-sample.git,APPLICATION_CONTEXT_DIR=''
```


## Test with curl command
```
$ oc get route
NAME       HOST/PORT                                             PATH      SERVICES   PORT       TERMINATION
java-app   java-app-spring-boot-s2i-demo.cloudapps.example.com             java-app   8080-tcp   

$ curl java-app-spring-boot-s2i-demo.cloudapps.example.com

Greetings from Spring Boot! - It is from Local
```


Now, you have spring boot application running on OpenShift Container Platform!

Move to [next tutorial](https://github.com/Jooho/spring-boot-ose-example/blob/master/docs/SPRING-TUTORIAL-4.md).

