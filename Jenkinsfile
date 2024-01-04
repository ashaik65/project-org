pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'ashaik65/newjs:latest'
        KUBE_NAMESPACE = 'nodejs'
        KUBE_DEPLOYMENT_NAME = 'nodejs-app'
    }
    
      stages {
        stage('Checkout') {
            steps {
                // Clone the Git repository
                script {
                    git branch: 'master', url: 'https://github.com/ashaik65/project-org.git'
                }
            }
        }


        stage('Build and Push Docker Image') {
            steps {
                script {
                    // Build Docker image
                    docker.build(DOCKER_IMAGE)

                    // Push Docker image to registry
                    docker.withRegistry('https://registry.hub.docker.com', 'docker') {
                        docker.image(DOCKER_IMAGE).push()
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Deploy to Kubernetes using kubectl
                    withKubeConfig([credentialsId: 'kubeconfig']) {
                        sh "kubectl apply -f deploy.yaml -n $KUBE_NAMESPACE"
                        sh "kubectl apply -f ingress.yaml -n $KUBE_NAMESPACE"
                    }
                }
            }
        }
    }
}
