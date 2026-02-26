pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "yourdockerhubusername/node-app"
    }

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/yourusername/node-app.git'
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:latest .'
            }
        }

        stage('Login to DockerHub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS')]) {
                        sh 'echo $PASS | docker login -u $USER --password-stdin'
                    }
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push $DOCKER_IMAGE:latest'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker stop node-app || true
                docker rm node-app || true
                docker run -d -p 80:3000 --name node-app $DOCKER_IMAGE:latest
                '''
            }
        }
    }
}
