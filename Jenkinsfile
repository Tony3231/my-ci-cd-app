pipeline {
    agent any

    environment {
        REMOTE_HOST = 'YOUR_APP_SERVER_PUBLIC_IP'
        REMOTE_USER = 'ubuntu'
        REMOTE_PATH = '/home/ubuntu'
        APP_JAR_NAME = 'app.jar' // or whatever your app builds to
    }

    stages {
        stage('Clone') {
            steps {
                echo 'Cloning the repository...'
                git branch: 'main', url: 'https://github.com/<your-username>/<your-repo>.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the application...'
                sh './gradlew build' // or mvn package / npm install etc.
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying to EC2 instance...'
                sh """
                scp -o StrictHostKeyChecking=no -i /home/ubuntu/QA.pem build/libs/${APP_JAR_NAME} ${REMOTE_USER}@${REMOTE_HOST}:${REMOTE_PATH}
                ssh -o StrictHostKeyChecking=no -i /home/ubuntu/QA.pem ${REMOTE_USER}@${REMOTE_HOST} "pkill -f ${APP_JAR_NAME}; nohup java -jar ${REMOTE_PATH}/${APP_JAR_NAME} > app.log 2>&1 &"
                """
            }
        }
    }
}
