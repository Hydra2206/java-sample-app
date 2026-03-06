# java-sample-app
created this java app by copilot to deploy in k8s cluster

steps
1) mvn clean package - to build a JAR file
2) created Dockerfile to build image for app
3) created deployment & service yml files
4) to upload artifact to nexus, added something in pom.xml
5) ~/.m2/settings.xml me jake nexus id, user, password add kiya
6) using docker run command to run nexus server
7) docker exec nexus cat /nexus-data/admin.password        using this command we can get initial pass
8) create a repo in nexus, user & roles to upload the artifact
9) abb joh admin user, pass hai usko settings.xml me update ka
10) nexus se artifact pull hora (artifact download karne me issue hai, isko check karna hai)
11) Deploying Sonarqube in Docker container instead of configuring in dedicated Instance.
12) deployed jenkins in docker container
