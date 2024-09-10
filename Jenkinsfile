pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'dockerdemo:latest' // Name of the Docker image to build
        EC2_USER = 'ubuntu' // Ubuntu's default user
        EC2_IP = 'your-ec2-ip-address' // Replace with your EC2 public IP
        SSH_CREDENTIALS_ID = 'ssh' // Your SSH credential ID
        GIT_REPO = 'https://github.com/KhagendraTiwari/dockerdemo.git' // Repository URL
    }

    stages {
        stage('Checkout') {
            steps {
                // Clone the GitHub repository
                git branch: 'main', url: "${GIT_REPO}"
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image from the Dockerfile in the repository
                    docker.build(DOCKER_IMAGE)
                }
            }
        }

        stage('Save Docker Image') {
            steps {
                script {
                    // Save the Docker image as a tar file
                    sh "docker save -o ${DOCKER_IMAGE}.tar ${DOCKER_IMAGE}"
                }
            }
        }

        stage('Transfer Docker Image to EC2') {
            steps {
                script {
                    // Transfer the Docker image tar file to the Ubuntu EC2 instance
                    sshagent(credentials: [SSH_CREDENTIALS_ID]) {
                        sh "scp -o StrictHostKeyChecking=no ${DOCKER_IMAGE}.tar ${EC2_USER}@${EC2_IP}:/home/${EC2_USER}/"
                    }
                }
            }
        }

        stage('Load and Run Docker Image on EC2') {
            steps {
                script {
                    // Load the Docker image on the EC2 instance and run it
                    sshagent(credentials: [SSH_CREDENTIALS_ID]) {
                        sh """
                        ssh -o StrictHostKeyChecking=no ${EC2_USER}@${EC2_IP} '
                        docker load -i /home/${EC2_USER}/${DOCKER_IMAGE}.tar &&
                        docker run -d -p 80:80 ${DOCKER_IMAGE}'
                        """
                    }
                }
            }
        }
    }

    post {
        always {
            cleanWs() // Clean workspace after build
        }
    }
}
