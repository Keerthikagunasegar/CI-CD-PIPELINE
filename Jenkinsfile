pipeline {
    agent any
    environment {
        DOCKER_CREDENTIALS_ID = 'docker-hub-credentials'
        KUBE_CONFIG_ID = 'kubeconfig'
    }
    stages {
        stage('Clone Repository') {
            steps {
                git credentialsId: 'github-credentials', url: 'https://github.com/Keerthikagunasegar/CI-CD-PIPELINE.git', branch: 'main'
            }
        }
        stage('Build and Test') {
            steps {
                sh 'docker build -t your-docker-repo/app:latest .'
                sh 'docker run --rm your-docker-repo/app:latest pytest tests/'
            }
        }
        stage('Push to Docker Hub') {
            steps {
                withDockerRegistry(credentialsId: DOCKER_CREDENTIALS_ID, url: 'https://index.docker.io/v1/') {
                    sh 'docker push your-docker-repo/app:latest'
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                withKubeConfig([credentialsId: KUBE_CONFIG_ID]) {
                    sh 'kubectl apply -f deployment.yaml'
                }
            }
        }
    }
}
