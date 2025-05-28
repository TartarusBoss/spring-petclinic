pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "tartarusboss/spring-petclinic"
        DOCKER_TAG = "latest"
        // Asegúrate de que tus credenciales Docker estén configuradas en Jenkins
        DOCKER_CREDENTIALS_ID = 'docker-hub-credentials-id' 
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/TartarusBoss/spring-petclinic.git', branch: 'main'
            }
        }
        stage('Build') {
            steps {
                sh './mvnw clean package -DskipTests'
            }
        }
        stage('Docker Build') {
            steps {
                // Verifica que docker esté disponible
                sh 'docker --version'
                // Construye la imagen
                sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
            }
        }
        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: DOCKER_CREDENTIALS_ID,
                                                  usernameVariable: 'DOCKER_USER',
                                                  passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }
        stage('Docker Push') {
            steps {
                sh "docker push ${DOCKER_IMAGE}:${DOCKER_TAG}"
            }
        }
    }
    post {
        failure {
            echo 'Build failed!'
        }
        success {
            echo 'Build and Docker image pushed successfully!'
        }
    }
}
