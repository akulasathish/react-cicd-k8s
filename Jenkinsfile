pipeline {
    agent any
    environment {
        DOCKER_HUB_CREDENTIALS = 'docker-hub-credentials'
        KUBE_CONFIG = '/root/.kube/config'
    }
    stages {
        stage('Clone Repository') {
            steps {
                git credentialsId: 'git-hub-credentials', url: 'https://github.com/akulasathish/fronendCICD.git', branch: 'master'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("akulasathish1997/react-deploy", "-f my-react-app/Dockerfile .")
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('', DOCKER_HUB_CREDENTIALS) {
                        docker.image("akulasathish1997/react-deploy").push()
                    }
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                withKubeConfig([credentialsId: 'kubectl-create-token', context: 'sathish.live']) {
                    sh 'kubectl apply -f my-react-app/deployment.yaml'
                    sh 'kubectl apply -f my-react-app/service.yaml'
                }
            }
        }
    }
}
