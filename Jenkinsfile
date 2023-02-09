pipeline {
    agent any
    stages {
        stage('Build Docker App') {
            steps {
                sh '''
                docker build -t eu.gcr.io/lbg-cloud-incubation/flask-app-alpine:latest -t eu.gcr.io/lbg-cloud-incubation/flask-app-alpine:build-$BUILD_NUMBER .
                '''
            }
        }
        stage('Push Image to gcr Hub') {
            steps {
                sh '''
                docker push eu.gcr.io/lbg-cloud-incubation/flask-app-alpine:latest
                docker push eu.gcr.io/lbg-cloud-incubation/flask-app-alpine:build-$BUILD_NUMBER
                '''
            }
        }
        stage('Deploy Application') {
            steps {
                if ("${GIT_BRANCH}" == 'origin/main') {
                sh '''
                ssh -i "~/.ssh/id_rsa" jenkins@35.246.18.45 << EOF
                docker stop alpine-app
                docker rm alpine-app
                docker rmi flask-app-alpine
                docker run -d -p 80:5500 --name alpine-app eu.gcr.io/lbg-cloud-incubation/flask-app-alpine:latest
                '''
                }else if("${GIT_BRANCH}" == 'origin/ps-flask-app-branch') {
                sh '''
                ssh -i "~/.ssh/id_rsa" jenkins@34.65.54.32 << EOF
                docker stop alpine-app
                docker rm alpine-app
                docker rmi flask-app-alpine
                docker run -d -p 80:5500 --name alpine-app eu.gcr.io/lbg-cloud-incubation/flask-app-alpine:latest
                '''
                }
            }
        }
    }
}