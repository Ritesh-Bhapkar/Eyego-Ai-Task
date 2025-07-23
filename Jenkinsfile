pipeline {
    agent any

    stages {

        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/Ritesh-Bhapkar/Eyego-Ai-Task.git'
            }
        }

        stage('Build & Push Docker Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-id',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    sh '''
                    docker build -t ritesh605/node-js-app:v1 .
                    echo $PASS | docker login -u $USER --password-stdin
                    docker push ritesh605/node-js-app:v1
                    '''
                }
            }
        }

        stage('Connect to EKS') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'aws-creds',
                    usernameVariable: 'AWS_KEY',
                    passwordVariable: 'AWS_SECRET'
                )]) {
                    sh '''
                    aws configure set aws_access_key_id $AWS_KEY
                    aws configure set aws_secret_access_key $AWS_SECRET
                    aws configure set default.region ap-southeast-2

                    aws eks update-kubeconfig --name ritesh-cluster4 --region ap-southeast-2
                    kubectl get nodes
                    '''
                }
            }
        }

        stage('Deploy to EKS') {
            steps {
                sh '''
                kubectl apply -f nodejs-app-deployment.yml
                '''
            }
        }
    }
}
