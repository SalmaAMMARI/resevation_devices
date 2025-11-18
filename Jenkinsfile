pipeline {
    agent any
    triggers {
        pollSCM('* * * * *')
    }
    
    stages {
        stage('Polling Diagnostic') {
            steps {
                echo '🎯 POLLING ACTIF - Vérification GitHub...'
                echo "📊 Build déclenché par: ${currentBuild.getBuildCauses()}"
                script {
                    // Commande Windows pour voir les commits
                    bat 'echo "🔍 Derniers commits:" && git log --oneline -3'
                }
            }
        }
        
        stage('Checkout') {
            steps {
                checkout scm
                echo "✅ Dernier commit: ${env.GIT_COMMIT}"
            }
        }
        
        stage('Build') {
            steps {
                echo '🔨 Construction en cours...'
                // Pour Maven sur Windows :
                bat 'mvn compile --version || echo "Maven non installé"'
            }
        }
        
        stage('Test') {
            steps {
                echo '🧪 Tests en cours...'
                bat 'echo "Simulation de tests Windows"'
                bat 'dir'  // Liste les fichiers
            }
        }
        
        stage('Fichiers du Workspace') {
            steps {
                echo '📁 Contenu du workspace:'
                bat 'dir /B'  // Liste les fichiers simplement
            }
        }
    }
    
    post {
        always {
            echo "🏁 Build ${env.BUILD_NUMBER} terminé - Polling actif"
            bat 'echo "Fin du build Windows"'
        }
    }
}