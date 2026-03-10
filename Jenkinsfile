pipeline {
  agent { label 'docker' }

  tools {
    maven 'Maven'

  }

  stages {
    stage('Checkout') {
      steps {
        sh 'echo passed'
        //git 'https://github.com/Hydra2206/java-sample-app.git'
      }
    }

    stage('Build & Upload to nexus'){
        steps {
            sh 'mvn clean package'
        }

    }

    stage('Static Code Analysis') {
      environment {
        SONAR_URL = "http://13.233.116.126:9000/"
      }
      steps {
        withSonarQubeEnv('SonarQube') {
          withCredentials([string(credentialsId: 'sonarqube', variable: 'SONAR_AUTH_TOKEN')]) {
            sh 'mvn sonar:sonar -Dsonar.token=$SONAR_AUTH_TOKEN -Dsonar.host.url=${SONAR_URL}'
        }
      }
    }
    }

    stage("Quality Gate") {
            steps {
              timeout(time: 1, unit: 'MINUTES') {
                waitForQualityGate abortPipeline: true
            }
    }
    }


    stage('Upload Artifact to Nexus') {
            steps {
                nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: '13.233.147.128:8081',
                    groupId: 'com.example',
                    version: '${BUILD_NUMBER}.0.0',
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
                apt update
                apt install docker.io -y
                docker build -t java-sample-app:${BUILD_NUMBER} .
                '''
            }
        }



       
}
}
