pipeline {
  agent any

  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds') // ID from Jenkins credentials
    KUBE_CONFIG = credentials('kubeconfig-id')             // Your kubeconfig file
  }

  stages {
    stage('Clone Repository') {
      steps {
        git 'https://github.com/Azeemakhanum66/microservices-k8s.git'
      }
    }

    stage('Build Docker Images') {
      parallel {
        stage('Build Frontend') {
          steps {
            script {
              sh 'docker build -t yourdockerhub/frontend ./frontend'
            }
          }
        }
        stage('Build ProductCatalog') {
          steps {
            script {
              sh 'docker build -t yourdockerhub/productcatalog ./productcatalogservice'
            }
          }
        }
        stage('Build AdService') {
          steps {
            script {
              sh 'docker build -t yourdockerhub/adservice ./adservice'
            }
          }
        }
      }
    }

    stage('Push Docker Images') {
      steps {
        script {
          docker.withRegistry('', 'dockerhub-creds') {
            sh 'docker push yourdockerhub/frontend'
            sh 'docker push yourdockerhub/productcatalog'
            sh 'docker push yourdockerhub/adservice'
          }
        }
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        withCredentials([file(credentialsId: 'kubeconfig-id', variable: 'KUBECONFIG')]) {
          sh 'kubectl apply -f k8s/frontend.yaml'
          sh 'kubectl apply -f k8s/productcatalogservice.yaml'
          sh 'kubectl apply -f k8s/adservice.yaml'
        }
      }
    }
  }
}
