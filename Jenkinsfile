pipeline {
  agent any
 
    
  stages {
    stage('Build Artifact') {
            steps {
		echo "test app"
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
//     stage('SonarQube Analysis') {
//       steps {
//         withSonarQubeEnv('SonarQube') {
//           sh "mvn  clean verify sonar:sonar -Dsonar.projectKey=java-app-1 -Dsonar.projectName='java-app-1' "
//         }
       
//   }
// }
// 	  stage('scanning docker image uing trivy'){
// 		  steps {
		  
// 		     sh "bash trivy-docker-image-scan.sh"
// 		  }
	   
// 	  }
    
//     stage('Vulnerability Scan - Docker') {
//       steps {
//           sh "mvn dependency-check:check"
// 	  }
//         post {
        
//           always {
//             dependencyCheckPublisher pattern: 'target/dependency-check-report.xml'
//           }
//         }
    
//     }
//     stage("qaulity gate"){
//       steps {
//         timeout(time: 2, unit: 'MINUTES') {
        
//             waitForQualityGate abortPipeline: true
//           }
//       }
      
      
    
//     }
//     stage('Build Docker Image') {
//       steps {
//         script {
// 	   sh "sudo mkdir -p /var/lib/jenkins/workspace/build-app-springboot/trivy "
//             sh "sudo chmod -R 777 /var/lib/jenkins/workspace/build-app-springboot/trivy"

//            dockerImage = docker.build("asmaayounis/java-app-1:${env.BUILD_NUMBER}")
//         }
//       }
//     }

    stage('Docker Hub Login') {
      steps {
	      withDockerRegistry([credentialsId: "docker-hub", url: "https://docker.io/"]) {
          sh 'printenv'
          sh 'sudo docker build -t asmaayounis/java-app-1:""$GIT_COMMIT"" .'
          sh 'docker push asmaayounis/java-app-1:""$GIT_COMMIT""'

      ' 
		   

          }
        }
      }
//      stage('K8S Deployment - DEV') {
//       steps {
      
//             withKubeConfig([credentialsId: 'kubeconfig']) {
//               sh "sed -i 's#replace#asmaayounis/java-app-1:${env.BUILD_NUMBER}#g' k8s_deployment_service.yaml"
//               sh "kubectl apply -f k8s_deployment_service.yaml"
//             }
//       }
//      }
//     }
// 	post {
//               always {
//                 junit 'target/surefire-reports/*.xml'
//                 jacoco execPattern: 'target/jacoco.exe'
//               }
//             }

//     }
