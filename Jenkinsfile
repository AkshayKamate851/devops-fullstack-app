pipeline {
    agent any

    environment {
        // Define environment variables
        DOCKER_REGISTRY = 'docker.io'  // Replace with your Docker registry URL
        DOCKER_REPO_BACKEND = 'akshaykamate/backend'
        DOCKER_REPO_FRONTEND = 'akshaykamate/frontend'
        KUBE_NAMESPACE = 'default'  // Replace with your Kubernetes namespace
        KUBE_CLUSTER_URL = 'https://127.0.0.1:6443'
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
                kubernetes {
                    cloud 'k3s'  // Use k3s cloud provider
                    deploy {
                        configs 'kubernetes/backend.yaml', 'kubernetes/frontend.yaml'
                        enableConfigSubstitution true
                        namespace "${KUBE_NAMESPACE}"
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
