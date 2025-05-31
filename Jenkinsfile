pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/AkashParmar7/samplehtml.git'
        IMAGE_NAME = 'yourdockerhubusername/html-page'
        TAG = 'latest'
    }

    stages {
        stage('Clone HTML Code') {
            steps {
                git url: "${REPO_URL}"
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:${TAG}")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    script {
                        docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-creds') {
                            docker.image("${IMAGE_NAME}:${TAG}").push()
                        }
                    }
                }
            }
        }

        stage('Deploy Docker Container') {
            steps {
                script {
                    // Stop and remove any running container with same name
                    sh "docker rm -f html-container || true"
                    // Run the new container
                    sh "docker run -d -p 8080:80 --name html-container ${IMAGE_NAME}:${TAG}"
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline completed"
        }
    }
}
