pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('dockerhub-creds') // Requiere que configures credencial en Jenkins
        DOCKER_IMAGE = "tartarusboss/spring-petclinic"
    }

    stages {
        stage('Clonar código') {
            steps {
                git 'https://github.com/TartarusBoss/spring-petclinic'
            }
        }

        stage('Compilar') {
            steps {
                sh './mvnw clean package -DskipTests'
            }
        }

        stage('Construir imagen Docker') {
            steps {
                sh "docker build -t $DOCKER_IMAGE ."
            }
        }

        stage('Login DockerHub') {
            steps {
                sh "echo $DOCKER_HUB_CREDENTIALS_PSW | docker login -u $DOCKER_HUB_CREDENTIALS_USR --password-stdin"
            }
        }

        stage('Publicar en DockerHub') {
            steps {
                sh "docker push $DOCKER_IMAGE"
            }
        }
    }
}
