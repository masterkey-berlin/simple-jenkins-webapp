// Jenkinsfile
pipeline {
    agent {
        docker {
            image 'python:3.11'
        }
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
                echo 'Code checkout complete.'
            }
        }
        stage('Build Application') {
            steps {
                echo 'Starting application build...'
                // Diese Befehle werden jetzt im Node.js Docker Container ausgeführt
                sh 'npm ci'
                sh 'npm run build'
                echo 'Application build finished.'
            }
        }
        stage('Build') {
            steps {
                echo 'Baue im Docker-Container...'
                sh 'python --version'
            }
        }
    }

    post {
        always {
            echo 'Pipeline run finished.'
        }
        success {
            echo 'Pipeline successfully completed!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}