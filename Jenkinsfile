pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials')
    }
    stages {
        stage('Clone') {
            steps {
                git url: 'https://github.com/oguzhanyildiz22/devops-pro4', branch: 'main'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t oguzhanyldz/demo:latest .'
            }
        }
        stage('Login to DockerHub') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('Push to DockerHub') {
            steps {
                sh 'docker push oguzhanyldz/demo:latest'
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/deployment.yaml'
            }
        }
        stage('Expose Service') {
            steps {
                sh 'kubectl apply -f k8s/service.yaml'
            }
        }
    }
}