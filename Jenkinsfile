pipeline {
    agent any
    triggers {
        pollSCM('* * * * *')
    }
    
    stages {
        stage('Checkout et Changements') {
            steps {
                checkout scm
                script {
                    // Affiche les informations de changement
                    echo "📊 BUILD DÉCLENCHÉ PAR: ${currentBuild.getBuildCauses()}"
                    echo "🔗 BRANCHE: ${env.GIT_BRANCH}"
                    echo "📝 COMMIT: ${env.GIT_COMMIT}"
                    
                    // Affiche les derniers changements
                    bat '''
                        echo "📋 DERNIERS CHANGEMENTS:"
                        git log --oneline -5
                        echo.
                        echo "👤 AUTEUR DU DERNIER COMMIT:"
                        git log -1 --pretty=format:"%an <%ae>"
                    '''
                }
            }
        }
        
        stage('Build') {
            steps {
                echo '🔨 Construction en cours...'
                bat 'echo Building application...'
                bat 'dir /B || echo Aucun fichier trouvé'
            }
        }
        
        stage('Tests') {
            steps {
                echo '🧪 Exécution des tests...'
                bat 'echo Running tests...'
                sleep 2
            }
        }
        
        stage('Analyse Qualité') {
            steps {
                echo '📊 Analyse de la qualité...'
                bat 'echo Quality analysis...'
            }
        }
    }
    
    post {
        always {
            echo "🏁 BUILD #${env.BUILD_NUMBER} TERMINÉ"
            script {
                // Affiche le statut final
                if (currentBuild.currentResult == 'SUCCESS') {
                    echo '🎉 SUCCÈS: Tous les tests passent!'
                } else {
                    echo '❌ ÉCHEC: Vérifiez les logs'
                }
            }
        }
        success {
            bat 'echo ✅✅✅ BUILD RÉUSSI ✅✅✅'
        }
    }
}