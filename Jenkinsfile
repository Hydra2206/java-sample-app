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

    stage('Build artifact'){
        steps {
            sh 'mvn clean package'
        }

    }

    stage('Static Code Analysis') {
      steps {
        withSonarQubeEnv('SonarQube') {
          withCredentials([string(credentialsId: 'sonarqube', variable: 'SONAR_AUTH_TOKEN')]) {
            sh 'mvn sonar:sonar -Dsonar.token=$SONAR_AUTH_TOKEN -Dsonar.host.url=${SonarQube}'
        }
      }
    }
    }


    stage('Upload artifact to Nexus') {
            steps {
                nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: '${NEXUS_IP}:8081',
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
                docker build -t java-sample-app:${BUILD_NUMBER} .
                '''
            }
        }

    stage('Push Docker Image') {
            steps {
                // docker login ${NEXUS_IP}:8082
                sh '''   
                docker tag java-sample-app:${BUILD_NUMBER} ${NEXUS_IP}:8082/java-sample-app:${BUILD_NUMBER}
                docker push ${NEXUS_IP}:8082/myapp:1.0
                '''
            }
        }



       
}
}
