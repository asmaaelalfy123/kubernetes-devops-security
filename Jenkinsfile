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
    stage('Docker Build and Push') {
      steps {
        withDockerRegistry([credentialsId: "docker-hub", url: "https://docker.io/"]) {
          sh 'printenv'
          sh 'sudo docker build -t asmaayounis/java-app-1:""$GIT_COMMIT"" .'
          sh 'docker push asmaayounis/java-app-1:""$GIT_COMMIT""'
        }
      }
    }
    }
}
