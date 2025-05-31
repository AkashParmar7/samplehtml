pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/AkashParmar7/samplehtml.git'
        IMAGE_NAME = 'akashparmar/html-page'  // Your DockerHub image name
        TAG = 'latest'
        CONTAINER_NAME = 'html-container'
        PORT = '8080'
    }

    stages {
        stage('Clone HTML Code') {
            steps {
                echo "Cloning repository from ${REPO_URL}"
                git branch: 'main', url: "${REPO_URL}"
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building Docker image ${IMAGE_NAME}:${TAG}"
                    sh 'docker --version' // Check if Docker is installed
                    sh "docker build -t ${IMAGE_NAME}:${TAG} ."
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    script {
                        echo "Logging in to Docker Hub"
                        sh "echo ${PASSWORD} | docker login -u ${USERNAME} --password-stdin"
                        echo "Pushing Docker image"
                        sh "docker push ${IMAGE_NAME}:${TAG}"
                        sh "docker logout"
                    }
                }
            }
        }

        stage('Deploy Docker Container') {
            steps {
                script {
                    echo "Stopping and removing old container (if exists)"
                    sh "docker rm -f ${CONTAINER_NAME} || true"
                    echo "Running new container ${CONTAINER_NAME} on port ${PORT}"
                    sh "docker run -d -p ${PORT}:80 --name ${CONTAINER_NAME} ${IMAGE_NAME}:${TAG}"
                }
            }
        }
    }

    post {
        always {
            echo "✅ Jenkins Pipeline completed."
        }
        failure {
            echo "❌ Jenkins Pipeline failed."
        }
    }
}
