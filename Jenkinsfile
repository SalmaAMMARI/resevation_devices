pipeline {
    agent any
    
    stages {
        // Étape 1: Récupérer le code
        stage('Clone') {
            steps {
                git branch: 'develop', 
                url: 'https://github.com/TON_USERNAME/NOM_DU_REPO.git'
            }
        }
        
        // Étape 2: Compiler
        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }
        
        // Étape 3: Tests
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        
        // Étape 4: Créer le package
        stage('Package') {
            steps {
                sh 'mvn package'
            }
        }
        
        // Étape 5: Analyse SonarQube
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
    }
    
    // Actions après le pipeline
    post {
        always {
            echo 'Pipeline terminé'
        }
        success {
            echo 'SUCCÈS !'
        }
        failure {
            echo 'ÉCHEC !'
        }
    }
}