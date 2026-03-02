pipeline {
    agent any

    environment {
        IMAGE_NAME = "rsraghavee/netflix-backend"
        CONTAINER_NAME = "netflix-backend-container"
        DOCKER_CREDS = "dockerhub-creds"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git 'https://github.com/rsraghavee/netflix-clone-mern.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                dir('backend') {
                    sh 'docker build -t $IMAGE_NAME:latest .'
                }
            }
        }

        stage('DockerHub Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: DOCKER_CREDS,
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                }
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push $IMAGE_NAME:latest'
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                docker stop $CONTAINER_NAME || true
                docker rm $CONTAINER_NAME || true

                docker run -d \
                  --name $CONTAINER_NAME \
                  -p 5000:5000 \
                  $IMAGE_NAME:latest
                '''
            }
        }
    }
}
