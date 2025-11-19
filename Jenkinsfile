pipeline {
    agent any
    triggers {
        pollSCM('H/5 * * * *')  // Poll GitHub every 5 minutes for changes
    }
    environment {
        DOCKER_IMAGE = 'salmaammari/reservation-devices'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
                echo "Build #${env.BUILD_NUMBER}"
                bat 'git log -1 --oneline'  // Show last commit
            }
        }
        stage('Docker Build') {
            steps {
                echo 'Building Docker Image...'
                script {
                    bat """
                        docker build -t ${DOCKER_IMAGE}:latest -t ${DOCKER_IMAGE}:${env.BUILD_NUMBER} .
                    """
                }
            }
        }
        stage('Docker Test') {
            steps {
                echo 'Testing Docker Image...'
                script {
                    bat """
                        echo "Testing if Docker image runs..."
                        docker run --rm ${DOCKER_IMAGE}:latest echo "Docker container test successful!"
                    """
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                echo 'Pushing to Docker Hub...'
                script {
                    withCredentials([usernamePassword(
                        credentialsId: 'docker-hub-credentials',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )]) {
                        bat """
                            echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin
                            docker push ${DOCKER_IMAGE}:latest
                            docker push ${DOCKER_IMAGE}:${env.BUILD_NUMBER}
                            echo "Images pushed successfully!"
                        """
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                script {
                    bat """
                        echo "Stopping existing container..."
                        docker stop reservation-app || echo "No running container found"
                        docker rm reservation-app || echo "No container to remove"
                        echo "Starting new container..."
                        docker run -d --name reservation-app -p 3000:3000 ${DOCKER_IMAGE}:latest
                        echo "Application deployed successfully!"
                    """
                }
            }
        }
    }
    post {
        success {
            echo 'Pipeline réussie! ✅'
            echo 'Docker Hub Repository: https://hub.docker.com/r/salmaammari/reservation-devices'
        }
        failure {
            echo 'Pipeline a échoué! ❌'
        }
    }
}