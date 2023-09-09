pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build and Push Docker Image') {
            steps {
                script {
                    docker.build('myapp:latest')
                    docker.withRegistry('https://registry.example.com', 'dockerhub-credentials') {
                        docker.image('myapp:latest').push()
                    }
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f myapp-deployment.yaml'
            }
        }
        stage('Test and Deploy if Successful') {
            steps {
                sh 'sh test.sh'
                sh 'kubectl apply -f myapp-service.yaml'
            }
        }
    }
}
