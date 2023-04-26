pipeline {
  agent any
  environment {
        DOCKER_REGISTRY = 'docker.io'
        DOCKER_IMAGE_NAME = 'java-app-1'
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub')
    }
    
  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar' //so that they can be downloaded later
            }
        }  
    stage('test app') {
            steps {
              sh "mvn test"
            }
            post {
              always {
                junit 'target/surefire-reports/*.xml'
                jacoco execPattern: 'target/jacoco.exe'
              }
            }
        }
    stage('Build') {
            steps {
                script {
                    docker.build("$DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:$BUILD_NUMBER")
                }
            }
        }
    stage('Docker Build and Push') {
      steps {
        
          script {
                    docker.withRegistry("$DOCKER_REGISTRY", "$DOCKER_HUB_CREDENTIALS") {
                     docker.image("$DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:$BUILD_NUMBER").push()
                    }
                }
       
        }
      }
    }
    }
