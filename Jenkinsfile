pipeline {
    agent any

    environment {
        GIT_REPO = 'https://github.com/SakshiBhavikatti/Cloud-automation.git'
        IMAGE_NAME = 'sakshi00/flask-app'
        AWS_REGION = 'ap-south-1'
    }

    stages {

        stage('Clone from GitHub') {
            steps {
                // withCredentials([string(credentialsId: 'github-pat', variable: 'GITHUB_TOKEN')]) {
                //     git credentialsId: 'github-pat', url: "https://github.com/SakshiBhavikatti/Cloud-automation.git", branch: 'main'
                // }
                git url: 'https://github.com/SakshiBhavikatti/Cloud-automation.git', branch: 'main'
            }
        }

        stage('Terraform Provisioning') {
            steps {
                dir('terraform') {
                    bat '''
                        terraform init
                        terraform apply -auto-approve
                    '''
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t flask-app ./app'
                bat 'docker tag flask-app ${IMAGE_NAME}'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'DockerHub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    bat '''
                        echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                        docker push ${IMAGE_NAME}
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                bat '''
                    kubectl apply -f deployment/deployment.yaml
                    kubectl apply -f deployment/service.yaml
                '''
            }
        }
    }

    post {
        always {
            dir('terraform') {
                bat '''
                    echo "Waiting 500 minutes before destroying resources..."
                    sleep 30000
                    terraform destroy -auto-approve
                '''
            }
        }
    }
}


