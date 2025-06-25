// Jenkinsfile
pipeline {
    agent any // Definiert, wo die Pipeline ausgeführt wird (hier: jeder verfügbare Agent)

    stages {
        stage('Checkout') { // Erste Stufe: Code aus dem SCM holen
            steps {
                // Dieser Step holt den Code basierend auf der SCM-Konfiguration im Jenkins-Job
                checkout scm
                echo 'Code checkout complete.'
            }
        }
        stage('Build Application') { // Zweite Stufe: Die Webanwendung bauen
            steps {
                echo 'Starting application build...'
                // Diese Befehle werden in der Workspace-Shell des Jenkins-Agenten ausgeführt
                // Stelle sicher, dass Node.js auf dem Agenten verfügbar ist
                // (Das offizielle jenkins/jenkins:lts Image hat oft Node.js, sonst muss man es installieren oder einen Docker Agent nutzen)
                sh 'npm ci'       // Abhängigkeiten installieren
                sh 'npm run build'  // Build-Skript ausführen
                echo 'Application build finished.'
            }
        }
    }

    post { // Aktionen, die nach allen Stages ausgeführt werden (optional)
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