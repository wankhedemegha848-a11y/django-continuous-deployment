

pipeline {
    agent any

    stages {

        stage('Git Checkout') {
            steps {
                echo 'Downloading Django application from GitHub'
                checkout scm
            }
        }

        stage('Docker Build') {
            steps {
                echo 'Building Django Docker image'
                sh 'docker compose build'
            }
        }

        stage('Deploy Application') {
            steps {
                echo 'Deploying Django application'
                sh 'docker compose up -d'
            }
        }

        stage('Verify Deployment') {
            steps {
                sh 'docker compose ps'
                sh 'curl -f http://localhost/'
            }
        }
    }

    post {
        success {
            echo 'Django Continuous Deployment Successful!'
        }

        failure {
            echo 'Deployment Failed! Check Console Output.'
        }
    }
}
