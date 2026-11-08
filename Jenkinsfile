pipeline {
    agent {
        label 'windows'
    }

    stages {
        stage('Windows Agent Check') {
            steps {
                echo 'Running on Windows Agent!'
                bat 'hostname'
                bat 'java --version'
            }
        }

        stage('Docker Check') {
            steps {
                bat 'docker --version'
                bat 'docker ps'
            }
        }

        stage('Kubernetes Check') {
            steps {
                bat 'kubectl version --client'
                bat 'kubectl get nodes'
                bat 'minikube status'
            }
        }

        stage('Deployment Verification') {
            steps {
                bat 'kubectl get deployments'
                bat 'kubectl get pods'
            }
        }
    }
}
