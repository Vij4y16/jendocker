pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('DOCKER_HUB_CREDENTIALS')
        IMAGE_NAME = 'fuzdocker/test'
        DOCKER_PATH = '/usr/local/bin/docker'
        CONTAINER_NAME = 'test'
        // Generate a unique tag based on the commit hash
        GIT_COMMIT_HASH = sh(script: 'git rev-parse --short HEAD', returnStdout: true).trim()
        // Define the tags for the image
        IMAGE_TAGS = "latest, ${GIT_COMMIT_HASH}" // Combine "latest" with the commit hash
    }

    stages {
        stage('Build') {
            steps {
                script {
                    // Build the Docker image with the specified tags
                    sh "${DOCKER_PATH} build -t ${IMAGE_NAME} --build-arg GIT_COMMIT_HASH=${GIT_COMMIT_HASH} -f Dockerfile ."
                    // Tag the image with additional tags
                    sh "${DOCKER_PATH} tag ${IMAGE_NAME} ${IMAGE_NAME}:${IMAGE_TAGS}"
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Run the Docker container with the "latest" tag
                    sh "${DOCKER_PATH} run -d -p 3000:3000 --name ${CONTAINER_NAME} ${IMAGE_NAME}:latest"
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    // Log in to Docker Hub using credentials
                    withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'DOCKER_HUB_CREDENTIALS', usernameVariable: 'DOCKER_HUB_USERNAME', passwordVariable: 'DOCKER_HUB_PASSWORD']]) {
                        sh "${DOCKER_PATH} login -u ${DOCKER_HUB_USERNAME} --password-stdin <<< ${DOCKER_HUB_PASSWORD}"
                        // Push the Docker image with the specified tags to Docker Hub
                        sh "${DOCKER_PATH} push ${IMAGE_NAME}:${IMAGE_TAGS}"
                    }
                }
            }
        }
    }
}
