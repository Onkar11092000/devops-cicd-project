pipeline {
    agent any

    tools {
        maven 'Maven-3.9'
    }

    environment {
        APP_NAME = "devops-cicd-project"
        IMAGE_NAME = "devops-cicd-project:v1"
        DOCKER_USER = "onkarborhade"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                url: 'https://github.com/Onkar11092000/devops-cicd-project.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t devops-cicd-project:v1 .'
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
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                        echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin

                        docker tag devops-cicd-project:v1 $DOCKER_USER/devops-cicd-project:v1

                        docker push $DOCKER_USER/devops-cicd-project:v1
                    '''
                }
            }
        }
    }

    post {
        success {
            echo "Pipeline Success - CI/CD Completed"
        }

        failure {
            echo "Pipeline Failed - Check Logs"
        }
    }
}