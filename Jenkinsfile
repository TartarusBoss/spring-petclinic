pipeline {
    agent any

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh './mvnw clean package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    // Verifica si docker está disponible
                    def dockerAvailable = sh(script: 'which docker', returnStatus: true) == 0
                    if (dockerAvailable) {
                        sh 'docker build -t myapp:latest .'
                    } else {
                        echo 'Docker no está disponible, se salta Docker Build'
                    }
                }
            }
        }

        stage('Docker Login') {
            when {
                expression {
                    sh(script: 'which docker', returnStatus: true) == 0
                }
            }
            steps {
                // Añade aquí login docker, solo si docker está disponible
                echo 'Haciendo login en Docker...'
                // sh 'docker login ...'
            }
        }

        stage('Docker Push') {
            when {
                expression {
                    sh(script: 'which docker', returnStatus: true) == 0
                }
            }
            steps {
                echo 'Haciendo push de la imagen Docker...'
                // sh 'docker push myapp:latest'
            }
        }
    }

    post {
        always {
            echo 'Build finalizado'
        }
        failure {
            echo 'Build falló!'
        }
    }
}
