pipeline {

  environment {
    registry = "docker.io/krishchan3/testpipeline/flask"
    registry_mysql = "docker.io/krishchan3/testpipeline/mysql"
    dockerImage = ""
  }

  agent any
    stages {
  
    stage('Checkout Source') {
      steps {
        git 'https://github.com/krishchan3/Docker-Project.git'
      }
    }

    stage('Build image') {
      steps{
        script {
 //         dockerImage = docker.build registry + ":$BUILD_NUMBER"
            dockerImage = docker.build("${registry}:${env.BUILD_NUMBER}")
        }
      }
    }

    stage('Push Image') {
	   environment {
	     registryCredential = 'dockerhublogin'
		 }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }

    stage('current') {
      steps{
        dir("${env.WORKSPACE}/mysql"){
          sh "pwd"
          }
      }
   }
   stage('Build mysql image') {
     steps{
       sh 'docker build -t "docker.io/krishchan3/testpipeline/mysql:$BUILD_NUMBER"  "$WORKSPACE"/mysql'
        sh 'docker push "docker.io/krishchan3/testpipeline/mysql:$BUILD_NUMBER"'
        }
      }
    stage('Deploy App') {
      steps {
        withCredentials([file(credentialsId: 'kubejenkins', variable: 'KUBECONFIG')]) {
          sh 'kubectl apply -f frontend.yml'
		}
      }
    }

  }

}
