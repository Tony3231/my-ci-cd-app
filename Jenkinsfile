pipeline {
    agent any

    environment {
        REMOTE_HOST = '13.203.230.78'       // Your EC2 instance public IP
        REMOTE_USER = 'ubuntu'
        REMOTE_PATH = '/home/ubuntu'
        APP_JAR_NAME = 'app-all.jar'        // Name of your built JAR
        SSH_KEY_PATH = '/var/lib/jenkins/QA.pem'
    }

    stages {
        stage('Clone') {
            steps {
                echo '�� Cloning the repository...'
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
                sh """
                    chmod 400 ${SSH_KEY_PATH}

                    echo '📦 Copying JAR file to EC2...'
                    scp -o StrictHostKeyChecking=no -i ${SSH_KEY_PATH} app/build/libs/${APP_JAR_NAME} ${REMOTE_USER}@${REMOTE_HOST}:${REMOTE_PATH}/

                    echo '🔄 Restarting application on EC2...'
ssh -o StrictHostKeyChecking=no -i ${SSH_KEY_PATH} ${REMOTE_USER}@${REMOTE_HOST} <<EOF
pkill -f ${APP_JAR_NAME} || true
nohup java -jar ${REMOTE_PATH}/${APP_JAR_NAME} > app.log 2>&1 &
exit 0
EOF
                """
            }
        }
    }
}

