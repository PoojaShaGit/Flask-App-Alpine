pipeline {
    agent any
    stages {
        stage('Build Docker App') {
            steps {
                sh '''
                docker build -t poojasdocker2023/flask-app-alpine:latest -t poojasdocker2023/flask-app-alpine:build-$BUILD_NUMBER .
                '''
            }
        }
        stage('Push Image to Docker Hub') {
            steps {
                sh '''
                docker push poojasdocker2023/flask-app-alpine:latest
                docker push poojasdocker2023/flask-app-alpine:build-$BUILD_NUMBER
                '''
            }
        }
        stage('Deploy Application') {
            steps {
                sh '''
                ssh -i "~/.ssh/id_rsa" jenkins@34.65.54.32 << EOF
                docker stop alpine-app
                docker rm alpine-app
                docker rmi flask-app-alpine
                docker run -d -p 80:5500 --name alpine-app poojasdocker2023/flask-app-alpine:latest
                '''
            }
        }
    }
}