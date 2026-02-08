pipeline {
    agent any
    environment {
        // Name your image (e.g., "my-app")
        IMAGE_NAME = "my-sec-app"
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Security Scan (File System)') {
            steps {
                // 1. Scan the code files BEFORE building
                // If "CRITICAL" bugs are found, exit code 1 (Fail pipeline)
                sh 'trivy fs . --format table --exit-code 1 --severity CRITICAL'
            }
        }

        stage('Build Docker Image') {
            steps {
                // 2. Build the image
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }

        stage('Security Scan (Docker Image)') {
            steps {
                // 3. Scan the built image
                // We permit "HIGH" but block "CRITICAL" vulnerabilities
                sh "trivy image --exit-code 1 --severity CRITICAL ${IMAGE_NAME}"
            }
        }
    }
}
