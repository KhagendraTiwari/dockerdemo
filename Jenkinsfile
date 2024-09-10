pipeline {
    agent any

    environment {
        // Define the Docker image tag
        DOCKER_IMAGE = 'your-docker-image-name:latest'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from SCM
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image using the Dockerfile
                    docker.build(DOCKER_IMAGE)
                }
            }
        }

        stage('Test Docker Image') {
            steps {
                script {
                    // Run the Docker image to verify it works correctly
                    docker.image(DOCKER_IMAGE).inside {
                        sh 'python app.py'
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Push the image to DockerHub (optional)
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {
                        docker.image(DOCKER_IMAGE).push()
                    }
                }
            }
        }
    }
}
