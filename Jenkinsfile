pipeline{
  agent any
  environment {
    
    VERSION = "${env.BUILD_ID}"


  }
  stages{

    stage('sonar quality check'){

      agent{
        docker {
          image 'maven'
          args '-u root'
        }
      }
      steps{
	script{
          withSonarQubeEnv(credentialsId: 'sonar-token') {
            sh 'mvn clean package sonar:sonar'
          } 
        }

      }
    }

  }
}
