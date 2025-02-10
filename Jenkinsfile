pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'mohamedathikr/ekart:latest'
        KUBE_CONFIG = "$HOME/.kube/config"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/Athik01/Ekart.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t $DOCKER_IMAGE .'
                }
            }
        }

        stage('Push Docker Image to Hub') {
            steps {
                script {
                    withDockerRegistry([credentialsId: 'mohamedathikr', url: 'https://index.docker.io/v1/']) {
                        sh 'docker push $DOCKER_IMAGE'
                    }
                }
            }
        }

        stage('Deploy to Minikube') {
            steps {
                script {
                    sh 'kubectl apply -f deploymentservice.yml'
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                script {
                    sh 'kubectl get pods'
                    sh 'kubectl get services'
                }
            }
        }
    }

    post {
        success {
            echo "Deployment Successful!"
        }
        failure {
            echo "Deployment Failed!"
        }
    }
}
