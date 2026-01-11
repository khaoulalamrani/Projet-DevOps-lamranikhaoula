pipeline {
    agent any
    
    tools {
        maven 'Maven'
    }
    
    stages {
        stage('Checkout') {
            steps {
                echo 'Récupération du code depuis GitHub...'
                git branch: 'main',
                    url: 'https://github.com/khaoulalamrani/Projet-DevOps-lamranikhaoula.git'
            }
        }
        
        stage('Build') {
            steps {
                echo 'Compilation du projet avec Maven...'
                sh 'mvn clean compile'
            }
        }
        
        stage('Test') {
            steps {
                echo 'Exécution des tests...'
                sh 'mvn test'
            }
        }
        
        stage('Package') {
            steps {
                echo 'Création du fichier JAR...'
                sh 'mvn package'
            }
        }
        
        stage('Archive') {
            steps {
                echo 'Archivage des artifacts...'
                archiveArtifacts artifacts: 'target/*.jar', 
                                 fingerprint: true
            }
        }
        
        stage('Deploy') {
            steps {
                echo 'Déploiement de l\'application...'
                sh 'java -jar target/*.jar'
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline exécuté avec succès! ✅'
        }
        failure {
            echo 'Le pipeline a échoué! ❌'
        }

        success {
            slackSend(
                channel: '#devops-notifications',
                color: 'good',
                message: "✅ Build réussi : ${env.JOB_NAME} #${env.BUILD_NUMBER} (<${env.BUILD_URL}|Voir les détails>)"
            )
        }
        failure {
            slackSend(
                channel: '#devops-notifications',
                color: 'danger',
                message: "❌ Build échoué : ${env.JOB_NAME} #${env.BUILD_NUMBER} (<${env.BUILD_URL}|Voir les détails>)"
            )
        }
    }
}
