pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                echo 'Pulling code...'
            }
        }
        stage('Build') {
            steps {
                sh 'docker --version'
                echo 'Building the application...'
            }
        }
    }
}
