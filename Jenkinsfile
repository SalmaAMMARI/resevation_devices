pipeline {
    agent any
    triggers {
        pollSCM('H/5 * * * *')  // Poll toutes les 5 minutes
    }
    environment {
        SONAR_PROJECT_KEY = 'reservation-devices'
        SONAR_PROJECT_NAME = 'Reservation Devices'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
                echo "Build #${env.BUILD_NUMBER}"
            }
        }
        stage('Build') {
            steps {
                echo 'Construction de l application...'
                bat 'echo "Build step"'
            }
        }
        stage('Test') {
            steps {
                echo 'Exécution des tests...'
                bat 'echo "Test step"'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv('SonarQube') {  // Le nom configuré dans Jenkins
                        bat """
                            sonar-scanner \
                            -D"sonar.projectKey=${SONAR_PROJECT_KEY}" \
                            -D"sonar.projectName=${SONAR_PROJECT_NAME}" \
                            -D"sonar.sources=." \
                            -D"sonar.host.url=http://localhost:9000" \
                            -D"sonar.sourceEncoding=UTF-8"
                        """
                    }
                }
            }
        }
    }
    post {
        success {
            echo '✅ Pipeline réussie!'
            echo '📊 Analyse SonarQube complétée: http://localhost:9000'
        }
        failure {
            echo '❌ Pipeline échouée!'
        }
        always {
            // Cette étape s'exécute toujours, même en cas d'échec
            echo '🔚 Fin de l exécution du pipeline'
        }
    }
}