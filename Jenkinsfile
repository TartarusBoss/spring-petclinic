pipeline {
    agent {
        docker {
            image 'docker:24.0.5-cli'  // Imagen oficial de Docker CLI
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }

    environment {
        DOCKER_BUILDKIT = '1'
    }

    stages {
        stage('Verificar Docker') {
            steps {
                echo 'Mostrando versión de Docker'
                sh 'docker version'

                echo 'Listando contenedores activos'
                sh 'docker ps'
            }
        }

        stage('Construir imagen') {
            steps {
                echo 'Construyendo imagen Docker personalizada'
                sh 'docker build -t mi-imagen:latest .'
            }
        }

        stage('Ejecutar contenedor') {
            steps {
                echo 'Ejecutando contenedor desde la imagen construida'
                sh 'docker run --rm mi-imagen:latest'
            }
        }
    }

    post {
        always {
            echo 'Pipeline finalizado'
        }
    }
}
