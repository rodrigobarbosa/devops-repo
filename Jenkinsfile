pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'node-app' // Nome da imagem Docker
    }
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/seu-usuario/devops-repo.git', branch: 'main'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}")
                }
            }
        }
        stage('Run Tests') {
            steps {
                script {
                    // Executa testes usando um contêiner temporário
                    docker.image("${DOCKER_IMAGE}").inside {
                        sh 'npm test'
                    }
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-password', variable: 'DOCKERHUB_PASS')]) {
                    script {
                        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                            docker.image("${DOCKER_IMAGE}").push("latest")
                        }
                    }
                }
            }
        }
    }
}
