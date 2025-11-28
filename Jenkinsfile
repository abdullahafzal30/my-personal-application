pipeline {
    agent any
    environment {
        DOCKER_REGISTRY = "localhost" // Replace with your Harbor registry hostname
        DOCKER_PROJECT = "my-project" // Replace with your Harbor project name
        IMAGE_NAME = "my-app"
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/abdullahafzal30/my-personal-application.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    def tag = "latest"
                    sh "docker build -t ${DOCKER_REGISTRY}/${DOCKER_PROJECT}/${IMAGE_NAME}:${tag} ."
                }
            }
        }
        stage('Push to Harbor') {
            steps {
                script {
                    def tag = "latest"
                    sh "docker login ${DOCKER_REGISTRY} -u admin -p Harbor12345"
                    sh "docker push ${DOCKER_REGISTRY}/${DOCKER_PROJECT}/${IMAGE_NAME}:${tag}"
                }
            }
        }
    }
    post {
        always {
            echo 'Cleaning up Docker resources...'
            sh 'docker rmi -f $(docker images -q)'
        }
    }
}
