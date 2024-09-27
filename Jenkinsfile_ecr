pipeline {
    agent any

    environment {
        PATH = "/usr/local/bin:/usr/bin:/bin:${env.PATH}"  // Add /usr/local/bin to PATH
        IMAGE_NAME = 'test'
        AWS_REGION = 'ap-south-1'
        AWS_ACCOUNT_ID = '450168357022'
        ECR_REPOSITORY = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${IMAGE_NAME}"
        DOCKER_PATH = '/usr/local/bin/docker'
        CONTAINER_NAME = 'test'
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

        stage('Run Unit Tests') {
            steps {
                script {
                    // Install npm dependencies and run unit tests using npm
                    sh 'npm install'
                    sh 'npm run test'  // This will run the tests defined in package.json
                }
            }
        }

        stage('Tag Docker Image for ECR') {
            steps {
                script {
                    // Tag the image for AWS ECR
                    sh "${DOCKER_PATH} tag ${IMAGE_NAME}:latest ${ECR_REPOSITORY}:latest"
                }
            }
        }

        stage('Login to AWS ECR') {
            steps {
                script {
                    // Login to AWS ECR
                    sh "aws ecr get-login-password --region ${AWS_REGION} | ${DOCKER_PATH} login --username AWS --password-stdin ${ECR_REPOSITORY}"
                }
            }
        }

        stage('Push to AWS ECR') {
            steps {
                script {
                    // Push the image to AWS ECR
                    sh "${DOCKER_PATH} push ${ECR_REPOSITORY}:latest"
                }
            }
        }
    }

    post {
        always {
            script {
                // Optionally, archive any test reports if generated
                // Adjust this based on your test output
                echo "Build completed"
            }
        }
    }
}
