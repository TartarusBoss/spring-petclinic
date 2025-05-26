pipeline {
    agent any

    environment {
        IMAGE_NAME = 'tartarusboss/spring-petclinic'
    }

    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/TartarusBoss/spring-petclinic'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:latest")
                }
            }
        }

        stage('Login to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    docker.image("${IMAGE_NAME}:latest").push()
                }
            }
        }
    }
}
