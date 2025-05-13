// pipeline {
    agent any

    stages {
        stage('Deploy to Local Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/deployment-service.yml'
                sh 'kubectl apply -f k8s/productcatalogservice-deployment.yaml'
                sh 'kubectl apply -f k8s/adservice-deployment.yaml'
                sh 'kubectl apply -f k8s/frontend-deployment.yaml'
            }
        }

        stage('Verify Deployment') {
            steps {
                sh 'kubectl get pods'
                sh 'kubectl get svc'
            }
        }
    }
}
