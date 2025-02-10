pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "mohamedathikr/ekart:latest"
        DOCKER_CREDENTIALS = "mohamedathikr"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', credentialsId: '890fb0fe-d7cc-4ce0-be01-6c51efe50fc1', url: 'https://github.com/Athik01/Ekart.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t ${DOCKER_IMAGE} -f docker/Dockerfile .'
                }
            }
        }

        stage('Push Docker Image to Hub') {
            steps {
                script {
                    withDockerRegistry([credentialsId: mohamedathikr, url: 'https://index.docker.io/v1/']) {
                        sh 'docker push ${DOCKER_IMAGE}'
                    }
                }
            }
        }

        stage('Deploy to Minikube') {
            steps {
                script {
                    sh """
                        minikube start
                        kubectl create deployment ekart-deployment --image=${DOCKER_IMAGE} --port=8070 || true
                        kubectl expose deployment ekart-deployment --type=NodePort --port=8070
                    """
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
}
