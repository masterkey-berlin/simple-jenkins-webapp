// Jenkinsfile
pipeline {
    agent {
        docker {
            image 'node:22-alpine' // Oder eine andere passende Node.js Version, z.B. node:20-alpine
            // args '-u root' // Nur hinzufügen, wenn du später Berechtigungsprobleme bekommst
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