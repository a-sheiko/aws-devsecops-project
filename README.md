# End-to-End DevSecOps Pipeline on AWS

This project demonstrates a secure CI/CD pipeline using Jenkins, Docker, Trivy, AWS ECR, and AWS EKS.

## Architecture
1. **Source Code:** Python Flask
2. **CI Server:** Jenkins (on EC2)
3. **Security:** Trivy (File System & Image scanning)
4. **Registry:** AWS ECR (Elastic Container Registry)
5. **Orchestrator:** AWS EKS (Kubernetes)

## How to Run
1. Update `deployment.yaml` with your ECR Image URI.
2. Update `Jenkinsfile` with your ECR URI and Cluster Name.
3. Push changes to GitHub.
4. Trigger Jenkins build.
