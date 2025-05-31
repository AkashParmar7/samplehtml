pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/AkashParmar7/samplehtml.git'
        IMAGE_NAME = 'akashparmar/html-page'  // Replace with your actual DockerHub username
        TAG = 'latest'
    }

    stages {
        stage('Clone HTML Code') {
            steps {
                git branch: 'main', url: "${REPO_URL}"
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${IMAGE_NAME}:${TAG} ."
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                 withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    script {
                        sh """
                            echo "$PASSWORD" | docker login -u "$USERNAME" --password-stdin
                            docker push akashparmar/html-page:latest
                        """
                    }
                }
            }
        }

        stage('Deploy Docker Container') {
            steps {
                script {
                    sh "docker rm -f html-container || true"
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
