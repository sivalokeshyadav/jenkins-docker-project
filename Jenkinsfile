pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "sivalokeshyadav/jenkins-demo:latest"
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

        stage('Run Tests') {
            steps {
                sh 'docker run --rm $DOCKER_IMAGE npm test'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withDockerRegistry([credentialsId: 'docker-hub-credentials', url: '']) {
                    sh 'docker push $DOCKER_IMAGE'
                }
            }
        }

        stage('Deploy Container') {
            steps {
                sh 'docker run -d -p 3000:3000 --name jenkins-app $DOCKER_IMAGE'
            }
        }
    }
}
