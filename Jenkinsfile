pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = 'docker-hub-credentials' // Replace with your actual Docker Hub credentials ID
        KUBE_CONFIG = '/var/lib/jenkins/.kube/config' // Updated path to your kubeconfig file
    }

    stages {
        stage('Cleanup Workspace') {
            steps {
                cleanWs() // Clean up the workspace
            }
        }
        stage('Clone Repository') {
            steps {
                git credentialsId: 'git-hub-credentials', url: 'https://github.com/akulasathish/react-cicd-k8s.git', branch: 'main' // Replace with your repo
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("akulasathish1997/react-deploy", "-f Dockerfile .") // Adjust the image name and Dockerfile if needed
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('', DOCKER_HUB_CREDENTIALS) {
                        docker.image("akulasathish1997/react-deploy").push() // Push the Docker image
                    }
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh '''
                    export KUBECONFIG=${KUBE_CONFIG} # Set the Kubeconfig environment variable
                    kubectl apply -f deployment.yaml # Apply the Kubernetes deployment
                    kubectl apply -f service.yaml # Apply the Kubernetes service
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment was successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
