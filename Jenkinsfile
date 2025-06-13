pipeline {
    agent any

    environment {
        REMOTE_HOST = '13.201.226.5'
        REMOTE_USER = 'ubuntu'
        REMOTE_PATH = '/home/ubuntu'
        APP_JAR_NAME = 'app-all.jar'
        PEM_PATH = '/var/lib/jenkins/QA.pem'
    }

    stages {
        stage('Clone') {
            steps {
                echo '📥 Cloning the repository...'
                git branch: 'main', url: 'https://github.com/Tony3231/my-ci-cd-app.git'
            }
        }

        stage('Build') {
            steps {
                echo '🔧 Building the application with shadowJar...'
                sh './gradlew clean :app:shadowJar'
            }
        }

        stage('Deploy') {
            steps {
                echo '🚀 Deploying to EC2 instance...'
                sh '''
                    chmod 400 $PEM_PATH
                    echo "📦 Copying JAR file to EC2..."
                    scp -o StrictHostKeyChecking=no -i $PEM_PATH app/build/libs/$APP_JAR_NAME $REMOTE_USER@$REMOTE_HOST:$REMOTE_PATH/
                '''
            }
        }
    }
}
