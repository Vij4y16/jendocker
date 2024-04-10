pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub-credentials-id')
        IMAGE_NAME = 'fuzdocker/test'
    }

    stages {
        stage('Build') {
            steps {
                script {
                    docker.build("$IMAGE_NAME:latest", "-f Dockerfile .")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_HUB_CREDENTIALS) {
                        docker.image("$IMAGE_NAME:latest").push()
                    }
                }
            }
        }
    }
}
