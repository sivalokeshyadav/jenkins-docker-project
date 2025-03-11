pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "sivalokeshyadav/jenkins-demo:latest"
        KUBE_CONFIG_PATH = "/home/jenkins/.kube/config"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/sivalokeshyadav/jenkins-docker-project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE ./app'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withDockerRegistry([credentialsId: 'docker-hub-credentials', url: '']) {
                    sh 'docker push $DOCKER_IMAGE'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/deployment.yaml'
                sh 'kubectl apply -f k8s/service.yaml'
            }
        }
    }
}
