pipeline {
    agent any
    
    environment {
        // Define environment variables
        DOCKER_REGISTRY = 'docker.io'  // Replace with your Docker registry URL
        DOCKER_REPO_BACKEND = 'akshaykamate/backend'
        DOCKER_REPO_FRONTEND = 'akshaykamate/frontend'
        KUBE_NAMESPACE = 'default'  // Replace with your Kubernetes namespace
    }
    
    stages {
        stage('Checkout') {
            steps {
                // Checkout source code from Git repository
                checkout scm
            }
        }
        
        stage('Build Backend Docker Image') {
            steps {
                script {
                    // Build backend Docker image
                    docker.build("${DOCKER_REGISTRY}/${DOCKER_REPO_BACKEND}:${BUILD_NUMBER}", './backend')
                }
            }
        }
        
        stage('Build Frontend Docker Image') {
            steps {
                script {
                    // Build frontend Docker image
                    docker.build("${DOCKER_REGISTRY}/${DOCKER_REPO_FRONTEND}:${BUILD_NUMBER}", './frontend')
                }
            }
        }
        
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Apply Kubernetes manifests
                    withKubeConfig(credentialsId: 'kube-config', serverUrl: 'https://127.0.0.1:6443') {
                        sh "kubectl apply -f kubernetes/backend.yaml -n ${KUBE_NAMESPACE}"
                        sh "kubectl apply -f kubernetes/frontend.yaml -n ${KUBE_NAMESPACE}"
                    }
                }
            }
        }
    }
    
    post {
        success {
            echo 'Deployment successful!'
            // Add any post-deployment tasks here
        }
        failure {
            echo 'Deployment failed!'
            // Add failure handling here if needed
        }
    }
}
