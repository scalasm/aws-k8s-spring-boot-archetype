# Spring Boot Maven Archetype for Microservices

This project is an archetype for scaffolding new microservice projects based on [Spring Boot](https://spring.io/projects/spring-boot) stack.

It leverages several tools that I find useful in my usual workflow:
* [Skaffold](https://skaffold.dev/) (0.15.0 or better) + Kustomize (included in Kubectl 1.14.x or newer)
* Maven 3.6.x + [Jib](https://github.com/GoogleContainerTools/jib) 2.6.x

With this you will get a RESTful endpoint with Spring Boot Web and readiness/liveness check configured, nothing more but
at least you can start tweaking things :)

I find it useful when starting my services from scratch, maybe somebody else may find it useful too!

Note that this archetype have been creating from [Hello World project repository](https://github.com/scalasm/aws-k8s-hello-world): 
you may want to take a look at it for more info about the stack and tools that I've used.

# How to use

1. Close this repository locally and launch the usual `mvn clean install`.
2. Change directory to a place where you want to create your microservice and run something like ():
```shell script
mvn archetype:generate -DarchetypeGroupId=me.marioscalas.archetypes \
    -DarchetypeArtifactId=aws-k8s-spring-boot-archetype \
    -DarchetypeVersion=0.0.1-SNAPSHOT \
    -DgroupId=me.marioscalas.sample \
    -DartifactId=cool-sample-microservice \
    -Dversion=0.0.1-SNAPSHOT
```
3. Build and run the microservice

Ensure you have your cluster available and run skaffold
```
skaffold run
```

Note: you can define custom profiles with Skaffold and define the Docker image repository to use. 

For example, on my laptop and using Docker for Desktop:
```
...
...
[INFO]
[INFO] Containerizing application to Docker daemon as cool-sample-microservice...
[WARNING] Base image 'gcr.io/distroless/java:11' does not use a specific image digest - build may not be reproducible
[INFO] Getting manifest for base image gcr.io/distroless/java:11...
[INFO] Building dependencies layer...
[INFO] Building resources layer...
[INFO] Building classes layer...
[INFO] Using base image with digest: sha256:28ec552405a92ed1a3767b81aaece5c48bd1b89dfb5f3c144b0e4cea4dd5ffa4
[INFO]
[INFO] Container entrypoint set to [java, -cp, /app/resources:/app/classes:/app/libs/*, me.marioscalas.sample.HelloWorldApplication]
[INFO] Loading to Docker daemon...
[INFO]
[INFO] Built image to Docker daemon as cool-sample-microservice
[INFO]
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  9.488 s
[INFO] Finished at: 2020-10-18T22:22:59+02:00
[INFO] ------------------------------------------------------------------------
[WARNING] The requested profile "personal" could not be activated because it does not exist.
Tags used in deployment:
 - cool-sample-microservice -> cool-sample-microservice:d91470fb45dc4b6e03e7f6602a5c6996b08fd1288074f76ed41a78c29be0f63f
Starting deploy...
 - configmap/cool-sample-microservice-cm created
 - service/cool-sample-microservice-svc created
 - deployment.apps/cool-sample-microservice created
Waiting for deployments to stabilize...
 - deployment/cool-sample-microservice: waiting for rollout to finish: 0 of 1 updated replicas are available...
 - deployment/cool-sample-microservice is ready.
Deployments stabilized in 24.7458471s
You can also run [skaffold run --tail] to get the logs
```

You can check that the microservice is up and running using Kubectl:
```
PS C:\src\medium-articles\cicd-kubernetes-aws\cool-sample-microservice> kubectl get pods
NAME                                        READY   STATUS    RESTARTS   AGE
cool-sample-microservice-57668bd97f-4qmgt   1/1     Running   0          30s
```

```
PS C:\src\medium-articles\cicd-kubernetes-aws\cool-sample-microservice> kubectl logs cool-sample-microservice-57668bd97f-4qmgt -f
               _  _          __    __               _      _
  /\  /\  ___ | || |  ___   / / /\ \ \  ___   _ __ | |  __| |
 / /_/ / / _ \| || | / _ \  \ \/  \/ / / _ \ | '__|| | / _` |
/ __  / |  __/| || || (_) |  \  /\  / | (_) || |   | || (_| |
\/ /_/   \___||_||_| \___/    \/  \/   \___/ |_|   |_| \__,_|
Using Spring Boot 2.3.4.RELEASE

20:23:02.638 [restartedMain] INFO  me.marioscalas.sample.HelloWorldApplication - Starting HelloWorldApplication on cool-sample-microservice-57668bd97f-4qmgt with PID 1 (/app/classes started by nobody in /)
20:23:02.640 [restartedMain] DEBUG me.marioscalas.sample.HelloWorldApplication - Running with Spring Boot v2.3.4.RELEASE, Spring v5.2.9.RELEASE
20:23:02.640 [restartedMain] INFO  me.marioscalas.sample.HelloWorldApplication - The following profiles are active: default
20:23:04.600 [restartedMain] INFO  me.marioscalas.sample.HelloWorldApplication - Started HelloWorldApplication in 2.488 seconds (JVM running for 3.162)
```
