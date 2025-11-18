pipeline {
    agent any
    triggers {
        pollSCM('* * * * *')  // ← Vérifie toutes les minutes
    }
    
    stages {
        stage('Diagnostic Polling') {
            steps {
                echo '🔍 DÉBUT DIAGNOSTIC POLLING'
                echo "📅 Heure actuelle: ${new Date()}"
                echo "🔗 Repository: ${env.GIT_URL}"
                echo "🔄 Branche: ${env.GIT_BRANCH}"
                echo "👤 Commit: ${env.GIT_COMMIT}"
                sh 'git log --oneline -3'  // Montre les 3 derniers commits
            }
        }
        
        stage('Checkout') {
            steps {
                checkout scm
                echo '✅ Code récupéré'
            }
        }
        
        // ... vos autres étapes
    }
}