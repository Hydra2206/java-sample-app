# java-sample-app
created this java app by copilot to deploy in k8s cluster

steps
1) mvn clean package - to build a JAR file                                                                           jenkins: 8080
2) created Dockerfile to build image for app                                                                         nexus: 8081
3) created deployment & service yml files                                                                            sonarqube: 9000
4) to upload artifact to nexus, added something in pom.xml
5) ~/.m2/settings.xml me jake nexus id, user, password add kiya
6) using docker run command to run nexus server
7) docker exec nexus cat /nexus-data/admin.password        using this command we can get initial pass
8) create a repo in nexus, user & roles to upload the artifact
9) abb joh admin user, pass hai usko settings.xml me update ka
10) nexus se artifact pull hora (artifact download karne me issue hai, isko check karna hai)
11) Deploying Sonarqube in Docker container instead of configuring in dedicated Instance.
12) deployed jenkins in docker container
13) installed sonar plugin in jenkins & configured a webhook in sonar for qualitygate check
14) created a ec2, add that vm as a node in jenkins & installed java, docker, maven in it using that node as agent to run pipeline
15) sonar & nexus ko individual ec2 server me run karra
16) successfully moved nexus_vol from my host m/c to nexus vm  || successfully moved sonar_vol from my host m/c to sonar vm
17) jenkins abhi bhi localhost me hi run hora hai
18) joh docker images build hora hai usko nexus repo me store karenge, uske liye nexus docker setup karra hu
19) Enable this in nexus (Docker bearer realm token) is a security token (usually a JSON Web Token, or JWT) used to authenticate operations with a Docker       registry, such as pulling or pushing images

Challanges

Problem - sonar server is deployed on docker container & it is running on localhost. Jenkins is using docker container as an agent to execute pipeline.      So in pipeline jenkins is trying to access sonar on localhost:9000 but getting connection refused bcoz,
pipeline is running inside container & when it is doing localhost it's happening for its container network not for my system network.

solution - Created a custome bridge network & connected that network to jenkins, sonarqube & nexus. Now they all are in the same network.


Problem - No such DSL method
solution - means required plugin needs to be installed