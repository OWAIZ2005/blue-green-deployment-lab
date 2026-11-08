pipeline {
    agent {
        label 'windows'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Source code fetched from GitHub'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t myapp:v2 ./green'
            }
        }

        stage('Load Image into Minikube') {
            steps {
                bat 'minikube image load myapp:v2'
            }
        }

        stage('Deploy Green') {
            steps {
                bat 'kubectl apply -f deployment-green.yaml'
                bat 'kubectl rollout status deployment/myapp-green --timeout=120s'
            }
        }

        stage('Verify Green') {
            steps {
                bat 'kubectl get pods -l color=green'
                bat 'kubectl get deployment myapp-green'
            }
        }

        stage('Manual Approval') {
            steps {
                input message: 'Green is healthy. Switch live traffic to Green?',
                      ok: 'Approve Traffic Switch'
            }
        }

        stage('Switch Traffic to Green') {
            steps {
                bat 'kubectl patch service myapp-service --type=merge --patch-file switch-green-patch.yaml'
                bat 'kubectl get endpoints myapp-service'
            }
        }
    }

    post {
        failure {
            echo 'Pipeline failed. Attempting rollback to Blue.'
            bat 'kubectl patch service myapp-service --type=merge --patch-file switch-blue-patch.yaml'
        }

        success {
            echo 'Blue-Green deployment completed successfully!'
        }
    }
}

