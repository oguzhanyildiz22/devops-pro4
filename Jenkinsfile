pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials')
    }
    stages {
        stage('Clone') {
            steps {
                git url: 'https://github.com/oguzhanyildiz22/devops-pro4', branch: 'main'
                sh 'ls -l k8s/'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Setup Minikube Docker Env') {
            steps {
                sh 'eval $(minikube docker-env)'
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
                sh '''
                kubectl config use-context minikube
                if [ ! -f k8s/deployment.yaml ]; then
                    echo "Hata: k8s/deployment.yaml bulunamadı!"
                    exit 1
                fi
                kubectl apply -f k8s/deployment.yaml
                '''
            }
        }
        stage('Expose Service') {
            steps {
                sh '''
                kubectl config use-context minikube
                if [ ! -f k8s/service.yaml ]; then
                    echo "Hata: k8s/service.yaml bulunamadı!"
                    exit 1
                fi
                kubectl apply -f k8s/service.yaml
                '''
            }
        }
    }
}