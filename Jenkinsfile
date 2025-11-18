pipeline {
    agent any
    
    stages {
        stage('Diagnostic Sécurité') {
            steps {
                echo '🛡️ DÉBUT DU DIAGNOSTIC SÉCURISÉ'
                echo '📊 Cette pipeline ne modifie rien'
            }
        }
        
        stage('Vérification Environnement') {
            steps {
                script {
                    echo "🔍 Informations système:"
                    echo "- Job: ${env.JOB_NAME}"
                    echo "- Build: ${env.BUILD_NUMBER}"
                    echo "- Workspace: ${env.WORKSPACE}"
                    
                    // Commandes safe pour diagnostic
                    bat '''
                        echo ✅ Vérification de base Windows:
                        echo 📅 Date: %date%
                        echo 🕐 Heure: %time%
                        echo 📁 Dossier: %cd%
                        echo.
                    '''
                }
            }
        }
        
        stage('Vérification Outils') {
            steps {
                script {
                    // Vérifie les outils sans les utiliser
                    bat '''
                        echo 🔧 Outils disponibles:
                        java -version 2>nul && echo ✅ Java trouvé || echo ❌ Java non trouvé
                        mvn --version 2>nul && echo ✅ Maven trouvé || echo ❌ Maven non trouvé
                        git --version 2>nul && echo ✅ Git trouvé || echo ❌ Git non trouvé
                        echo.
                    '''
                }
            }
        }
        
        stage('Vérification Projet Existant') {
            steps {
                script {
                    echo '📂 Analyse du projet existant...'
                    bat '''
                        echo 📋 Structure des fichiers:
                        dir /B 2>nul || echo ℹ️  Aucun fichier trouvé
                        echo.
                        
                        echo 📝 Fichiers de configuration:
                        if exist pom.xml (
                            echo ✅ Fichier Maven (pom.xml) détecté
                            type pom.xml | findstr "<artifactId>" | head -1 2>nul && echo 📦 Projet Maven identifié
                        ) else (
                            echo ℹ️  Aucun fichier Maven détecté
                        )
                        
                        if exist package.json (
                            echo ✅ Fichier Node.js (package.json) détecté
                        )
                        
                        if exist build.gradle (
                            echo ✅ Fichier Gradle (build.gradle) détecté
                        )
                        echo.
                    '''
                }
            }
        }
        
        stage('Test Lecture Seule') {
            steps {
                script {
                    echo '📖 Test de lecture seule...'
                    bat '''
                        echo 🔒 Test permissions lecture:
                        dir /B > list_files.txt 2>nul && echo ✅ Peut lire les fichiers || echo ❌ Problème lecture
                        if exist list_files.txt (
                            type list_files.txt 2>nul && echo ✅ Peut lire le contenu || echo ❌ Problème lecture contenu
                            del list_files.txt 2>nul
                        )
                        echo.
                    '''
                }
            }
        }
        
        stage('Rapport Final') {
            steps {
                script {
                    echo '📊 RAPPORT DE DIAGNOSTIC COMPLET'
                    echo '✅ Aucune modification effectuée'
                    echo '✅ Environnement analysé avec succès'
                    echo '✅ Projet existant préservé'
                    echo '🛡️ Diagnostic terminé en sécurité'
                }
            }
        }
    }
    
    post {
        always {
            echo "🏁 Diagnostic #${env.BUILD_NUMBER} terminé"
            echo '📋 Résumé disponible dans les logs'
            echo '🔧 Prêt pour la configuration réelle'
        }
        success {
            echo '🎉 Diagnostic réussi - Environnement prêt!'
        }
    }
}