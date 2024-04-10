pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('DOCKER_HUB_CREDENTIALS')
        IMAGE_NAME = 'fuzdocker/test'
        DOCKER_PATH = '/usr/local/bin/docker'
    }

    stages {
        stage('Build') {
            steps {
                script {
                    sh "${DOCKER_PATH} build -t ${IMAGE_NAME}:latest -f Dockerfile ."
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    // Retrieve Docker Hub credentials
                    withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'DOCKER_HUB_CREDENTIALS', usernameVariable: 'DOCKER_HUB_USERNAME', passwordVariable: 'DOCKER_HUB_PASSWORD']]) {
                        // Log in to Docker Hub using --password-stdin
                        sh "echo ${DOCKER_HUB_PASSWORD} | ${DOCKER_PATH} login -u ${DOCKER_HUB_USERNAME} --password-stdin"
                        // Push the Docker image to Docker Hub
                        sh "${DOCKER_PATH} push ${IMAGE_NAME}:latest"
                    }
                }
            }
        }
    }
}
