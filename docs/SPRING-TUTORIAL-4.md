# Use Spring Cloud Kubernetes

This tutorial make the sample application to get string from ConfigMap of OpenShift Container Platform instread of application.properties file.


## Add Spring Cloud/Spring Cloud Kubernetes dependencies to pom.xml
Please add following values to proper location in pom.xml

*spring-cloud-kubenetes-sample/pom.xml*
```
vi spring-cloud-kubenetes-sample/pom.xml

<properties>
        <spring-cloud.version>Brixton.SR7</spring-cloud.version>
        <spring-cloud-kubernetes.version>0.1.4</spring-cloud-kubernetes.version>
</properties>
...
...
<dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>${spring-cloud.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
...
...
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-context</artifactId>
</dependency>
<dependency>
    <groupId>io.fabric8</groupId>
    <artifactId>spring-cloud-starter-kubernetes</artifactId>
    <version>${spring-cloud-kubernetes.version}</version>
</dependency>

```

## Update application.properties
```
hello.message=Local
spring.application.name=hello      <===== Add
```

## Push source to github repositories
```
git add .
git commit -m "update source"
git push
```

## Create ConfigMap
```
$ oc create -f spring-cloud-kubenetes-sample/helloConfigMap.yaml

$ oc get configmap 
NAME      DATA      AGE
hello     1         2s

```
## Add permission to Service Account
```
oc policy add-role-to-user view system:serviceaccount:$(oc project -q):default -n $(oc project -q)
```


## Redeploy the application 
```
$ oc start-build java-app
```

## Test with curl command
```
$ oc get route
NAME       HOST/PORT                                             PATH      SERVICES   PORT       TERMINATION
java-app   java-app-spring-boot-s2i-demo.cloudapps.example.com             java-app   8080-tcp   

$ curl java-app-spring-boot-s2i-demo.cloudapps.example.com

Greetings from Spring Boot! - It is from Kubernetes ConfigMap 
```


Now, you have spring boot application which use ConfigMap running on OpenShift Container Platform!

Mission Complete!!
