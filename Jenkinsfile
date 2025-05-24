pipeline {
    agent any
    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/your-repo/flask-app.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t flask-app .'
            }
        }
        stage('Push to DockerHub') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-pass', variable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u your-username --password-stdin'
                    sh 'docker tag flask-app your-username/flask-app:latest'
                    sh 'docker push your-username/flask-app:latest'
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f deployment/deployment.yaml'
                sh 'kubectl apply -f deployment/service.yaml'
            }
        }
    }
}