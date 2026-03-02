pipeline {
    agent any

    environment {
        IMAGE_NAME = "rsraghavee/netflix-backend"
        DOCKER_CREDS = "dockerhub-creds"
    }

    stages {

        stage('Clone Code') {
            steps {
                git 'https://github.com/askhan963/netflix-clone-mern.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                dir('backend') {
                    sh 'docker build -t $IMAGE_NAME:latest .'
                }
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push $IMAGE_NAME:latest'
            }
        }
    }
}
