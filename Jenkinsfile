pipeline {
    agent any
    triggers {
        pollSCM('* * * * *')
    }
    environment {
        SONAR_TOKEN = '76d7effbcdd4314edbd16cf917881c3c3139c680'
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
                echo 'Compilation...'
                bat 'echo Building app...'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                echo 'Vraie analyse SonarCloud...'
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
                echo 'Build Docker...'
                bat 'echo "SIMULATION: docker build"'
            }
        }
    }
    post {
        success {
            echo 'Pipeline reussie!'
            echo 'SonarCloud: https://sonarcloud.io/project/overview?id=salmaammari_reservation-devices'
        }
    }
}