pipeline {
  agent any

  environment {
    DOCKERHUB_CREDENTIALS = 'dockerhub-creds'  // Tu ID de credenciales DockerHub en Jenkins
    IMAGE_NAME = 'tartarusboss/spring-petclinic'  // Cambia "tuusuario" por tu usuario DockerHub
  }

  stages {
    stage('Build Image') {
      steps {
        script {
          sh "docker build -t ${IMAGE_NAME}:latest ."
        }
      }
    }

    stage('Login to DockerHub') {
      steps {
        script {
          docker.withRegistry('', DOCKERHUB_CREDENTIALS) {
            echo 'Logged in to DockerHub'
          }
        }
      }
    }

    stage('Push Image') {
      steps {
        script {
          sh "docker push ${IMAGE_NAME}:latest"
        }
      }
    }
  }
}
