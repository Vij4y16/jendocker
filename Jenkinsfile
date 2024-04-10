pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('DOCKER_HUB_CREDENTIALS')
        IMAGE_NAME = 'fuzdocker/test'
        DOCKER_PATH = '/usr/local/bin/docker' // Replace this with the actual path to Docker executable
    }

    stages {
        stage('Build') {
            steps {
                script {
                    // Build the Docker image
                    sh "${DOCKER_PATH} build -t ${IMAGE_NAME}:latest -f Dockerfile ."
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    // Log in to Docker Hub
                    withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'DOCKER_HUB_CREDENTIALS', usernameVariable: 'DOCKER_HUB_USERNAME', passwordVariable: 'DOCKER_HUB_PASSWORD']]) {
                        sh "${DOCKER_PATH} login -u ${DOCKER_HUB_USERNAME} -p ${DOCKER_HUB_PASSWORD}"
                    }
                    // Push the Docker image to Docker Hub
                    sh "${DOCKER_PATH} push ${IMAGE_NAME}:latest"
                }
            }
        }
    }
}
