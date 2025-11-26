pipeline {
    agent any
    triggers {
        pollSCM('H/5 * * * *')  // Poll toutes les 5 minutes
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
    }
    post {
        success {
            echo '✅ Pipeline réussie!'
        }
        failure {
            echo '❌ Pipeline échouée!'
        }
    }
}