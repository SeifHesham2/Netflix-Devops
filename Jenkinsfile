pipeline {
    environment {
        NodejsHome = tool "myNode"
        dockerHome = tool "myDocker"
        TMDB_V3_API_KEY = credentials('TMDB_V3_API_KEY')
        PATH = "${dockerHome}/bin:${NodejsHome}/bin:${PATH}"
    }
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Install Dependencies') {
            steps {
                script {
                    echo 'Installing npm dependencies...'
                    sh 'npm install'
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    echo 'Building Docker image...'
                    def imageTag = "${env.BUILD_NUMBER}"
                    dockerImage = docker.build("seifseddik120/netflix-2024:${imageTag}")
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    echo 'Pushing Docker image to DockerHub...'
                    docker.withRegistry('https://index.docker.io/v1/', 'DockerHub') {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Start Minikube') {
            steps {
                script {
                    echo 'Starting Minikube...'
                    sh 'minikube start --driver=docker'
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    echo 'Deploying to Kubernetes...'
                    sh 'envsubst < deployment.yaml | kubectl apply -f -'
                    sh 'kubectl apply -f service.yaml'
                }
            }
        }
        stage('Verify Deployment') {
            steps {
                script {
                    echo 'Verifying deployment...'
                    sh 'kubectl get pods'
                    sh 'kubectl get services'
                }
            }
        }
    }
}
