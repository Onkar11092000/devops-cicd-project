pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                deleteDir()
                git branch: 'main',
                url: 'https://github.com/Onkar11092000/devops-cicd-project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t devops-cicd-project:v1 .'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run -d -p 8080:8080 --name app devops-cicd-project:v1 || true'
            }
        }
    }

    post {
        success {
            echo "Pipeline Success 🚀"
        }
        failure {
            echo "Pipeline Failed ❌"
        }
    }
}