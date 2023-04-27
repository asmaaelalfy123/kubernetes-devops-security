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
//     stage('Mutation Tests - PIT') {
//       steps {
//         sh "mvn org.pitest:pitest-maven:mutationCoverage"
//       }
//       post {
//         always {
        
//           pitmutation mutationStatsFile: '**/target/pit-reports/**/mutations.xml'
//         }
//       }
//     }
    stage('SonarQube Analysis') {
      
      def mvn = tool 'mvn';
      withSonarQubeEnv() {
        sh "${mvn}/bin/mvn clean verify sonar:sonar -Dsonar.projectKey=java-app-1 -Dsonar.projectName='java-app-1'"
    
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
     stage('K8S Deployment - DEV') {
      steps {
      
            withKubeConfig([credentialsId: 'kubeconfig']) {
              sh "sed -i 's#replace#asmaayounis/java-app-1:${env.BUILD_NUMBER}#g' k8s_deployment_service.yaml"
              sh "kubectl apply -f k8s_deployment_service.yaml"
            }
      }
     }
    }

    }
