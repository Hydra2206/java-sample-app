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

    stage('Build & Upload to nexus'){
        steps {
            sh 'cd java-sample-app && mvn clean package'
        }

    }

    stage('Static Code Analysis') {
      environment {
        SONAR_URL = "http://127.0.0.0:9000/"
      }
      steps {
        withCredentials([string(credentialsId: 'sonarqube', variable: 'SONAR_AUTH_TOKEN')]) {
          sh 'mvn sonar:sonar -Dsonar.token=$SONAR_AUTH_TOKEN -Dsonar.host.url=${SONAR_URL}'
        }
      }
    }

    stage("Quality Gate") {
            steps {
                waitForQualityGate abortPipeline: true
            }
    }


    stage('Upload Artifact to Nexus') {
            steps {
                nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: '127.0.0.0:8081',
                    groupId: 'com.example',
                    version: '1.0.0',
                    repository: 'java-artifacts',
                    credentialsId: 'nexus-cred',
                    artifacts: [
                        [
                            artifactId: 'java-sample-app',
                            classifier: '',
                            file: 'target/java-sample-app-1.0.0.jar',
                            type: 'jar'
                        ]
                    ]
                )
            }
    }

    stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t java-sample-app:v1 .
                '''
            }
        }



       
}
}
