pipeline {
    agent any
    environment {
        // Your specific ECR Repository URI
        ECR_REGISTRY = "959776247993.dkr.ecr.us-east-1.amazonaws.com/my-sec-app"
        IMAGE_TAG = "latest"
    }
    stages {
        stage('Checkout') {
            steps { checkout scm }
        }
        
        stage('Security Scan (File System)') {
            steps {
                // Scanning code before build
                sh 'trivy fs . --format table --exit-code 0 --severity CRITICAL'
            }
        }

        stage('Build Docker Image') {
            steps {
                // Build the image using the ECR URI as the name
                sh "docker build -t $ECR_REGISTRY:$IMAGE_TAG ."
            }
        }

        stage('Security Scan (Docker Image)') {
            steps {
                // Scan the built image
                sh "trivy image --exit-code 0 --severity CRITICAL $ECR_REGISTRY:$IMAGE_TAG"
            }
        }

        stage('Push to ECR') {
            steps {
                script {
                    // 1. Log in to ECR (AWS CLI uses the IAM Role automatically!)
                    sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin $ECR_REGISTRY"
                    
                    // 2. Push the image to AWS
                    sh "docker push $ECR_REGISTRY:$IMAGE_TAG"
                }
            }
        
        }
        stage('Deploy to EKS') {
            steps {
                script {
                    // 1. Authenticate kubectl with the new cluster
                    sh "aws eks update-kubeconfig --name devsecops-cluster --region us-east-1"
                    
                    // 2. Apply the deployment file (Deploy the App)
                    sh "kubectl apply -f deployment.yaml"
                    
                    // 3. Verify it worked (Optional but good for logs)
                    sh "kubectl get pods"
                }
            }
        }
    }
}
