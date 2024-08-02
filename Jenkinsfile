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
        stage('Build') {
            steps {
                sh "npm install"
                env.VITE_APP_TMDB_V3_API_KEY = env.TMDB_V3_API_KEY
                
                    }        }
        stage('Build Docker Image') {
            steps {
                script {
                    echo 'Building Docker image...'
                    dockerImage = docker.build("seifseddik120/netflix-2024:${env.BUILD_NUMBER}")
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    echo 'Pushing Docker image to registry...'
                    docker.withRegistry('', 'DockerHub') {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Start Minikube') {
            steps {
                script {
                    echo 'Starting Minikube...'
                    // Ensure minikube is properly configured with any needed options
                    sh 'minikube start --driver=docker'
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    echo 'Deploying to Kubernetes...'
                    // Ensure environment variables are set for envsubst
                    sh 'envsubst < deployment.yaml | kubectl apply -f deployment.yaml'
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
