pipeline {
    agent {
        kubernetes {
            inheritFrom 'jenkins-kaniko-agent'
        }
    }

    environment {
        IMAGE_NAME = "docker.io/shreeshail050/trivy-demo"
        IMAGE_TAG  = "1.0"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build & Push Image using Kaniko') {
            steps {
                container('kaniko') {
                    sh '''
                      echo "=== KANIKO BUILD STARTED ==="

                      /kaniko/executor \
                        --context /workspace \
                        --dockerfile Dockerfile \
                        --destination ${IMAGE_NAME}:${IMAGE_TAG} \
                        --destination ${IMAGE_NAME}:latest \
                        --verbosity=info

                      echo "=== KANIKO BUILD COMPLETED ==="
                    '''
                }
            }
        }

        stage('Trivy Image Scan') {
            steps {
                sh '''
                  echo "=== TRIVY IMAGE SCAN STARTED ==="

                  trivy image \
                    --exit-code 1 \
                    --severity CRITICAL \
                    ${IMAGE_NAME}:${IMAGE_TAG}

                  echo "=== TRIVY IMAGE SCAN COMPLETED ==="
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline succeeded. Image built and scanned successfully.'
        }
        failure {
            echo '❌ Pipeline failed due to critical vulnerabilities or build issues.'
        }
    }
}
