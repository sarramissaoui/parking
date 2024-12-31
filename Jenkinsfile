pipeline {
    agent any
    environment {
        DOCKER_REGISTRY = "docker.io"
        USERNAME = "your-dockerhub-username" // Replace with your Docker Hub username
        BACKEND_IMAGE = "${DOCKER_REGISTRY}/${USERNAME}/backend:latest"
        FRONTEND_IMAGE = "${DOCKER_REGISTRY}/${USERNAME}/frontend:latest"
    }
    stages {
        stage('Build Backend') {
            steps {
                script {
                    echo "Building Backend Docker Image..."
                    sh "docker build -t ${BACKEND_IMAGE} ./backend"
                }
            }
        }
        stage('Build Frontend') {
            steps {
                script {
                    echo "Building Frontend Docker Image..."
                    sh "docker build -t ${FRONTEND_IMAGE} ./frontend"
                }
            }
        }
        stage('Push Images') {
            steps {
                script {
                    echo "Logging into Docker Hub..."
                    withDockerRegistry([credentialsId: 'docker-hub-credentials']) {
                        echo "Pushing Backend Image to Docker Hub..."
                        sh "docker push ${BACKEND_IMAGE}"
                        
                        echo "Pushing Frontend Image to Docker Hub..."
                        sh "docker push ${FRONTEND_IMAGE}"
                    }
                }
            }
        }
    }
    post {
        success {
            echo 'Docker images have been successfully built and pushed to Docker Hub.'
        }
        failure {
            echo 'There was an issue with building or pushing the Docker images.'
        }
        always {
            cleanWs()
        }
    }
}
