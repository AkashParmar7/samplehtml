pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Cloning Repository...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'No build needed for static HTML. Skipping build step.'
            }
        }

        stage('Test') {
            steps {
                echo 'You can add HTML validation tools here (e.g., htmlhint).'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying static site...'
                // You can copy to an S3 bucket, or deploy to a web server
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
