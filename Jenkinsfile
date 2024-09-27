pipeline {
    agent any

    environment {
        PATH = "/usr/local/bin:/usr/bin:/bin:${env.PATH}"  // Add /usr/local/bin to PATH
        IMAGE_NAME = 'test'
        DOCKER_PATH = '/usr/local/bin/docker'
        EC2_USER = 'ec2-user'  // Assuming the default EC2 user (adjust if needed)
        EC2_HOST = '13.232.232.55'  // Your EC2 instance public IP
        EC2_KEY = '/Volumes/Data/AWS/AWS_Keys/AWS_Keys/Keys/stockX.pem'  // Path to your EC2 key file
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh "${DOCKER_PATH} build -t ${IMAGE_NAME}:latest -f Dockerfile ."
                }
            }
        }

        stage('Save Docker Image Locally') {
            steps {
                script {
                    // Save the Docker image to a tar file
                    sh "${DOCKER_PATH} save -o ${IMAGE_NAME}.tar ${IMAGE_NAME}:latest"
                }
            }
        }

        stage('Copy Docker Image to EC2') {
            steps {
                script {
                    // Use scp to copy the Docker image to the EC2 instance
                    sh "scp -i ${EC2_KEY} ${IMAGE_NAME}.tar ${EC2_USER}@${EC2_HOST}:~/"
                }
            }
        }
    }

    post {
        always {
            script {
                echo "Build and image transfer completed"
            }
        }
    }
}
