pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                // Clonar el repositorio testing-b
                checkout([$class: 'GitSCM', 
                          branches: [[name: '*/main']], // Cambia si usas otra rama
                          userRemoteConfigs: [[url: 'https://github.com/codingIA-a/testing-b.git']]
                ])
            }
        }
        stage('Run Bandit') {
            steps {
                script {
                    // Ejecutar Bandit solo en test.py
                    def result = sh(script: 'python3 -m bandit  /testing-b/test.py ', returnStatus: true)
                    if (result != 0) {
                        error 'Bandit found issues in the code'
                    }
                }
            }
        }
        stage('Publish Report') {
            steps {
                // Publicar el informe de Bandit
                publishHTML(target: [
                    reportName: 'Bandit Report',
                    reportDir: '.',
                    reportFiles: 'bandit-report.json',
                    alwaysLinkToLastBuild: true,
                    keepAll: true
                ])
            }
        }
    }
}
