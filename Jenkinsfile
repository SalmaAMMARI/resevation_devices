pipeline {
    agent any
    triggers {
        pollSCM('H/5 * * * *')  // Poll GitHub every 5 minutes for changes
    }
    environment {
        SONAR_TOKEN = '76d7effbcdd4314edbd16cf917881c3c3139c680'
        DOCKERHUB_CREDENTIALS = credentials('docker-hub-credentials')
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
        stage('Build') {
            steps {
                echo 'Compilation...'
                bat 'echo Building app...'
                // Add your actual build commands here
                // bat 'mvn compile' or 'npm install' etc.
            }
        }
        stage('SonarQube Analysis') {
            steps {
                echo 'Running SonarCloud Analysis...'
                script {
                    bat """
                        sonar-scanner ^
                        -Dsonar.projectKey=salmaammari_reservation-devices ^
                        -Dsonar.organization=salmaammari ^
                        -Dsonar.sources=. ^
                        -Dsonar.host.url=https://sonarcloud.io ^
                        -Dsonar.login=%SONAR_TOKEN%
                    """
                }
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
            echo 'SonarCloud Report: https://sonarcloud.io/project/overview?id=salmaammari_reservation-devices'
            echo 'Docker Hub Repository: https://hub.docker.com/r/salmaammari/reservation-devices'
            emailext (
                subject: "SUCCESS: Pipeline ${env.JOB_NAME} - Build #${env.BUILD_NUMBER}",
                body: "The build was successful! Check reports at:\nSonarCloud: https://sonarcloud.io/project/overview?id=salmaammari_reservation-devices\nDocker Hub: https://hub.docker.com/r/salmaammari/reservation-devices",
                to: "your-email@example.com"
            )
        }
        failure {
            echo 'Pipeline a échoué! ❌'
            emailext (
                subject: "FAILED: Pipeline ${env.JOB_NAME} - Build #${env.BUILD_NUMBER}",
                body: "The build failed. Please check Jenkins logs for details.",
                to: "your-email@example.com"
            )
        }
        always {
            echo 'Nettoyage des ressources Docker...'
            bat """
                docker system prune -f
                echo "Cleanup completed"
            """
        }
    }
}