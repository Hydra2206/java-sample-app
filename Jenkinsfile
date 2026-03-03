pipeline {
  agent {
    docker {
      image 'mittu7/my-maven-docker-agent:v1'
      args '--user root -v /var/run/docker.sock:/var/run/docker.sock' // mount Docker socket to access the host's Docker daemon
    }
  }

  stages {
    stage('Checkout') {
      steps {
        sh 'echo passed'
        git 'https://github.com/Hydra2206/java-sample-app.git'
      }
    }

    stage('Build & Deploy to nexus'){

    }
}
