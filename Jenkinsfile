pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building the Docker image...'
                sh 'docker build -t node-app .'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                // Add actual test commands if any
                sh 'echo "No tests configured."'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying the app...'
                sh 'docker run -d -p 3000:3000 --name node-app-container node-app'
            }
        }
    }
}
