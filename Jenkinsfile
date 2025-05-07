pipeline {
    agent any  // Runs on any available agent

    environment {
        // Set your Docker Hub credentials and repository information
        DOCKER_IMAGE = "tobilobaojo57/my-node-app"  // Your Docker Hub username and repo name
        DOCKER_TAG = "latest"  // Tag your image as 'latest' or use a version tag
        DOCKER_REGISTRY = "https://index.docker.io/v1/"  // Docker Hub registry URL
        DOCKER_CREDENTIALS = "dockerhub-creds"  // Jenkins credential ID for Docker Hub
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Checkout the code from GitHub repository
                    checkout scm
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image with 'latest' tag
                    echo "Building Docker Image with tag: ${DOCKER_TAG}..."
                    sh """
                        docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} .
                    """
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    // Login to Docker Hub using Jenkins credentials
                    echo "Logging into Docker Hub..."
                    withCredentials([usernamePassword(credentialsId: DOCKER_CREDENTIALS, usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh """
                            docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD} ${DOCKER_REGISTRY}
                        """
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Push the Docker image to Docker Hub with 'latest' tag
                    echo "Pushing Docker Image to Docker Hub..."
                    sh """
                        docker push ${DOCKER_IMAGE}:${DOCKER_TAG}
                    """
                }
            }
        }
    }

    post {
        success {
            echo "Docker Image successfully pushed to Docker Hub with tag: ${DOCKER_TAG}!"
        }
        failure {
            echo "The pipeline failed. Please check the logs for errors."
        }
    }
}

