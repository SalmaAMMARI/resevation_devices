pipeline {
    agent any
    triggers {
        pollSCM('* * * * *')
    }
    
    stages {
        stage('Test Simple') {
            steps {
                echo '🚀 Démarrage pipeline Windows...'
                bat 'echo ✅ Étape 1 réussie'
            }
        }
        
        stage('Checkout') {
            steps {
                checkout scm
                echo '📦 Code récupéré depuis GitHub'
            }
        }
        
        stage('Workspace Info') {
            steps {
                echo '📁 Analyse du workspace...'
                script {
                    // Méthode safe pour lister les fichiers
                    bat '''
                        echo Listing des fichiers:
                        dir
                        echo.
                        echo Répertoire courant:
                        cd
                    '''
                }
            }
        }
        
        stage('Git Info') {
            steps {
                script {
                    // Vérifie si Git est disponible
                    bat 'git --version || echo "Git non trouvé"'
                    bat 'git log --oneline -1 || echo "Impossible de voir les commits"'
                }
            }
        }
    }
    
    post {
        always {
            echo "🏁 Build ${env.BUILD_NUMBER} terminé avec succès!"
        }
    }
}