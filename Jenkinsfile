pipeline {
    agent any

    environment {
        IMAGE_NAME = 'fuzdocker/test'
        DOCKER_PATH = '/usr/local/bin/docker'
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

        stage('Run Docker Container') {
            steps {
                script {
                    // Run the Docker container
                    sh "${DOCKER_PATH} run -d -p 3000:3000 ${IMAGE_NAME}:latest"
                }
            }
        }
    }
}
