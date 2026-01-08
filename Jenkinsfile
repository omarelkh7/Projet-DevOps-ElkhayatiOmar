pipeline {
    agent any

    environment {
        IMAGE_NAME = "devops-app"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'dev',
                    url: 'https://github.com/omarelkh7/Projet-DevOps-ElkhayatiOmar.git'
            }
        }

        stage('Build & Test') {
            steps {
                sh 'mvn clean test package'
            }
        }


        stage('Archive') {
            steps {
                archiveArtifacts artifacts: '**/target/*.jar'
            }
        }

        
    }

    post {
        success {
            slackSend(
                channel: '#jenkins',
                message: "✅ Déploiement réussi : ${env.JOB_NAME} #${env.BUILD_NUMBER}"
            )
        }
        failure {
            slackSend(
                channel: '#jenkins',
                message: "❌ Échec du pipeline : ${env.JOB_NAME} #${env.BUILD_NUMBER}"
            )
        }
    }
}
