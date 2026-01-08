pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t trivy-demo:1.0 .'
            }
        }

        stage('Trivy Image Scan') {
            steps {
                sh '''
                  trivy image                     --exit-code 1                     --severity CRITICAL                     trivy-demo:1.0
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Build passed. No critical vulnerabilities found.'
        }
        failure {
            echo '❌ Build failed due to critical vulnerabilities.'
        }
    }
}
