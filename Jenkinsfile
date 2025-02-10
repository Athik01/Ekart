pipeline {
    agent any

    environment {
        IMAGE_NAME = "ekart-app"
        NAMESPACE = "ekart-ns"
    }

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/Athik01/Ekart.git'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Push Image to Minikube') {
            steps {
                script {
                    sh 'eval $(minikube docker-env)'
                    sh 'docker tag $IMAGE_NAME $IMAGE_NAME:latest'
                }
            }
        }

        stage('Deploy to Minikube') {
            steps {
                sh 'kubectl create namespace $NAMESPACE || true'
                sh 'kubectl apply -f k8s/deployment.yaml -n $NAMESPACE'
                sh 'kubectl apply -f k8s/service.yaml -n $NAMESPACE'
            }
        }

        stage('Expose Application') {
            steps {
                sh 'minikube service ekart-service -n $NAMESPACE --url'
            }
        }
    }
}
