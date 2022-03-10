pipeline {
  agent any

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar' //aaa
            }
        }
    
    stage('Docker Build and Push') {
      steps {
        withDockerRegistry([credentialsId: "docker-hub", url: ""]) {
          sh 'printenv'
          sh 'docker build -t jhong40/numeric-app:""$GIT_COMMIT"" .'
          sh 'docker push jhong40/numeric-app:""$GIT_COMMIT""'
        }
      }
    }       
    
    
    stage('SonarQube') {
      steps {
        sh 'mvn sonar:sonar -Dsonar.projectKey=numeric-application -Dsonar.host.url=http://160.1.38.192:9000 -Dsonar.login=6880161baf8b301da7e1f232225dc981c407fb8b'
      }
    }     
    
    
   stage('Kubernetes Deployment - DEV') {
      steps {
        withKubeConfig([credentialsId: 'kubeconfig']) {
          sh "sed -i 's#replace#jhong40/numeric-app:${GIT_COMMIT}#g' k8s_deployment_service.yaml"
          sh "kubectl apply -f k8s_deployment_service.yaml"
        }
      }
    }
    
    
    
    
    }
}
