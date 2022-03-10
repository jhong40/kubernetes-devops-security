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
    
    
    }
}
