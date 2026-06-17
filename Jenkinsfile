pipeline {
    agent any

    environment {
        IMAGE_NAME = "devops-cicd-project"
        DOCKER_USER = "YOUR_DOCKERHUB_USERNAME"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                url: 'https://github.com/Onkar11092000/devops-cicd-project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t $IMAGE_NAME:v1 ."
            }
        }

        stage('Run Container (Test)') {
            steps {
                sh '''
                docker rm -f app || true
                docker run -d -p 8080:8080 --name app devops-cicd-project:v1
                '''
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh '''
                    echo $PASS | docker login -u $USER --password-stdin
                    docker tag devops-cicd-project:v1 $USER/devops-cicd-project:v1
                    docker push $USER/devops-cicd-project:v1
                    '''
                }
            }
        }
    }

    post {
        success {
            echo "🚀 Full CI/CD Pipeline Success"
        }
    }
}