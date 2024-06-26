pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('DOCKER_HUB_CREDENTIALS')
        IMAGE_NAME = 'fuzdocker/test'
        DOCKER_PATH = '/usr/local/bin/docker'
        CONTAINER_NAME = 'test'
    }

    stages {
        stage('Build') {
            steps {
                script {
                    sh "${DOCKER_PATH} build -t ${IMAGE_NAME}:latest -f Dockerfile ."
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    sh "${DOCKER_PATH} run -d -p 3000:3000 --name ${CONTAINER_NAME} ${IMAGE_NAME}:latest"
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    // Retrieve Docker Hub credentials
                    withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'DOCKER_HUB_CREDENTIALS', usernameVariable: 'DOCKER_HUB_USERNAME', passwordVariable: 'DOCKER_HUB_PASSWORD']]) {
                        // Log in to Docker Hub using --password-stdin
                        sh "echo ${DOCKER_HUB_PASSWORD}"
                        sh "echo ${DOCKER_HUB_USERNAME}"
                        // Push the Docker image to Docker Hub
                        sh "${DOCKER_PATH} push ${IMAGE_NAME}:latest"
                    }
                }
            }
        }
    }
}
