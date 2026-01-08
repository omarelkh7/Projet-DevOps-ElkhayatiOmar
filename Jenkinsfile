pipeline {
    agent any

    environment {
        IMAGE_NAME = "devops-java-app"
    }

    stages {

        stage('Build') {
            steps {
                sh '''
                mkdir -p build
                javac -d build devops-app/src/com/devops/projet/App.java
                '''
            }
        }

        stage('Test / Run') {
            steps {
                sh '''
                java -cp build com.devops.projet.App
                '''
            }
        }

        stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'build/**'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Deploy') {
            when {
                expression { currentBuild.currentResult == "SUCCESS" }
            }
            steps {
                sh '''
                docker rm -f java-container || true
                docker run -d --name java-container $IMAGE_NAME
                '''
            }
        }
    }

    post {
        success {
            slackSend(
                channel: '#jenkins',
                message: "✅ Pipeline Java réussi : ${env.JOB_NAME} #${env.BUILD_NUMBER}"
            )
        }
        failure {
            slackSend(
                channel: '#jenkins',
                message: "❌ Pipeline Java échoué : ${env.JOB_NAME} #${env.BUILD_NUMBER}"
            )
        }
    }
}
