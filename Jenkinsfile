pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/Tony3231/my-ci-cd-app.git'

            }
        }

        stage('Build') {
            steps {
                echo 'Building the app...'
                // Add build commands here if needed
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying to app server...'
                // Example: use SSH to copy files or restart service
            }
        }
    }
}

