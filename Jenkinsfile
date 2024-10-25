pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', 
                          branches: [[name: '*/main']], 
                          userRemoteConfigs: [[url: 'https://github.com/codingIA-a/testing-b.git']]
                ])
            }
        }
        stage('Set Up Environment') {
            steps {
                script {
                    // Crear un entorno virtual y activar
                    sh 'python3 -m venv venv'
                    sh '. venv/bin/activate && pip install bandit'
                }
            }
        }
        stage('Run Bandit') {
            steps {
                script {
                    // Ejecutar Bandit en test.py
                    def result = sh(script: '. venv/bin/activate && bandit -r test.py -f json -o bandit-report.json', returnStatus: true)
                    if (result != 0) {
                        error 'Bandit found issues in the code'
                    }
                }
            }
        }
        stage('Publish Report') {
            steps {
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
