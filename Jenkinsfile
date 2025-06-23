pipeline {
  agent any

  environment {
    REGISTRY = "krishchan3"
    FLASK_IMAGE = "krishchan3/flask-app"
    MYSQL_IMAGE = "krishchan3/mysql-custom"
    REGISTRY_CREDENTIALS = "dockerhublogin"

  }

  stages {
  
    stage('Checkout') {
      steps {
        git 'https://github.com/krishchan3/Docker-Project.git'
      }
    }
    
    stage('Build Flask Image') {
      steps {
        script {
          docker.build("${FLASK_IMAGE}:${env.BUILD_NUMBER}")
        }
      }
    }

   stage('Build MySQL Image') {
      steps {
        script {
          docker.build("${MYSQL_IMAGE}:${env.BUILD_NUMBER}")
        }
      }
    }

   stage('Push to Docker Hub') {
      steps {
        script {
          docker.withRegistry('https://registry.hub.docker.com', REGISTRY_CREDENTIALS) {
          docker.image("${FLASK_IMAGE}:${env.BUILD_NUMBER}").push()
          docker.image("${MYSQL_IMAGE}:${env.BUILD_NUMBER}").push()
          }
        }
      }
   }
   
   stage('Update K8s Deployment YAML') {
      steps {
        script {
          // Replace image name in nginx-deploy.yml with the new tag
          sh """
            sed -i 's|image: ${FLASK_IMAGE}:.*|image: ${FLASK_IMAGE}:${env.BUILD_NUMBER}|' frontend.yaml
            sed -i 's|image: ${MYSQL_IMAGE}:.*|image: ${MYSQL_IMAGE}:${env.BUILD_NUMBER}|' frontend.yaml
            cat frontend.yaml
          """
        }
      }
    }

   stage('Deploy App') {
      steps {
               withCredentials([file(credentialsId: 'kubejenkins', variable: 'KUBECONFIG')]) {
               sh 'kubectl apply -f frontend.yaml'
	    }
      }
    }
  }
}




















    
