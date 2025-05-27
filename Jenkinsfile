pipeline {
  agent {
    docker {
      image 'docker:20.10.24' // imagen con docker client instalado
      args '-v /var/run/docker.sock:/var/run/docker.sock'
    }
  }
  environment {
    DOCKERHUB_CREDENTIALS = 'dockerhub-creds'
    IMAGE_NAME = 'tartarusboss/spring-petclinic'
  }
  stages {
    stage('Build Image') {
      steps {
        sh "docker build -t ${IMAGE_NAME}:latest ."
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
        sh "docker push ${IMAGE_NAME}:latest"
      }
    }
  }
}
