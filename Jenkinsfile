pipeline {
  agent any
 
    
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
    stage('Build Docker Image') {
      steps {
        script {
           dockerImage = docker.build("asmaayounis/java-app-1:${env.BUILD_NUMBER}")
        }
      }
    }

    stage('Docker Hub Login') {
      steps {
        script {
           withDockerRegistry([ credentialsId: "docker-hub", url: "" ]) {
             dockerImage.push()
        }
          }
        }
      }
    }

    }
    
