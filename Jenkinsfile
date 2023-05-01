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
    stage('Quality Gate Status'){
      steps{
        script{
          waitForQualityGate abortPipeline: false, credentialsId: 'sonar-token'
          
        }

      }
    }
    stage('Docker build and docker pushed to Nexus repo') {
      steps {
        script{
          withCredentials([string(credentialsId: 'nexus_password', variable: 'nexus_credential')]) {
            sh '''
               docker build -t 192.168.1.8:8083/spring:${VERSION} .
               docker login -u admin -p admin 192.168.1.8:8083
               docker push 192.168.1.8:8083/spring:${VERSION}
               docker rmi 192.168.1.8:8083/spring:${VERSION}
            '''
          }        



        }


      }

    }
  }
    post {
      always {
        mail bcc: '', body: "<br>Project: ${env.JOB_NAME} <br>Build  Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}", cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '', subject: "${currentBuild.result} CI: Project name -> ${env.JOB_NAME}", to: "thapa.krishna0111@gmail.com";
      }
    }


}
