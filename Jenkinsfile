pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'tartarusboss/spring-petclinic'
        DOCKER_TAG = 'latest'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']], 
                    extensions: [],
                    userRemoteConfigs: [[
                        url: 'https://github.com/tartarusboss/spring-petclinic.git',
                        credentialsId: 'github-token', 
                    ]],
                    timeout: 10 
                ])
            }
        }

        stage('Maven Build') {
            agent {
                docker {
                    image 'maven:3.9.6-eclipse-temurin-17'
                    args '-v $HOME/.m2:/root/.m2'
                    reuseNode true 
                }
            }
            steps {
                sh 'mvn clean install'
            }
        }
    }
}
