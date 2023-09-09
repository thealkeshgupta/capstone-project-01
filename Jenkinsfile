pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Docker Build and Tag') {
           steps {
                sh 'docker build -t myapp:latest .' 
                sh 'docker tag nginxtest alkeshgupta/myapp:latest'
                sh 'docker tag nginxtest alkeshgupta/myapp:$BUILD_NUMBER' 
          }
        }
        stage('Docker Image') {
            steps {
                script {
                    withDockerRegistry([ credentialsId: "dockerhub-credentials", url: "" ]) {
                        sh  'docker push alkeshgupta/myapp:latest'
                        sh  'docker push alkeshgupta/myapp:$BUILD_NUMBER' 
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
