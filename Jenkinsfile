pipeline {
    agent any

    environment {
<<<<<<< HEAD
        REMOTE_HOST = '13.232.216.202'       // Your EC2 instance public IP
        REMOTE_USER = 'ubuntu'
        REMOTE_PATH = '/home/ubuntu'
        APP_JAR_NAME = 'app-all.jar'         // Correct JAR name from shadowJar
        SSH_KEY_PATH = '/var/lib/jenkins/QA.pem'
=======
        REMOTE_HOST = '13.232.216.202' // Your EC2 instance public IP
        REMOTE_USER = 'ubuntu'
        REMOTE_PATH = '/home/ubuntu'
        APP_JAR_NAME = 'app.jar' // This must match the actual JAR output
>>>>>>> 8aabe0ab384b7f07c49a61bff948ef2e44aeb7af
    }

    stages {
        stage('Clone') {
            steps {
                echo '🔄 Cloning the repository...'
                git branch: 'main', url: 'https://github.com/Tony3231/my-ci-cd-app.git'
            }
        }

        stage('Build') {
            steps {
<<<<<<< HEAD
                echo '🔧 Building the application with shadowJar...'
                sh './gradlew clean :app:shadowJar'
=======
                echo '🔧 Building the application...'
                sh './gradlew clean build'
>>>>>>> 8aabe0ab384b7f07c49a61bff948ef2e44aeb7af
            }
        }

        stage('Deploy') {
            steps {
                echo '🚀 Deploying to EC2 instance...'
                sh """
<<<<<<< HEAD
                    chmod 400 ${SSH_KEY_PATH}

                    echo '📦 Copying JAR file to EC2...'
                    scp -o StrictHostKeyChecking=no -i ${SSH_KEY_PATH} app/build/libs/${APP_JAR_NAME} ${REMOTE_USER}@${REMOTE_HOST}:${REMOTE_PATH}/

                    echo '🔄 Restarting application on EC2...'
                    ssh -o StrictHostKeyChecking=no -i ${SSH_KEY_PATH} ${REMOTE_USER}@${REMOTE_HOST} '
                        pkill -f ${APP_JAR_NAME} || true
                        nohup java -jar ${REMOTE_PATH}/${APP_JAR_NAME} > app.log 2>&1 &
                    '
=======
                    chmod 400 /var/lib/jenkins/QA.pem

                    echo '📦 Copying JAR file to EC2...'
                    scp -o StrictHostKeyChecking=no -i /var/lib/jenkins/QA.pem app/build/libs/${APP_JAR_NAME} ${REMOTE_USER}@${REMOTE_HOST}:${REMOTE_PATH}/

                    echo '🔄 Restarting application on EC2...'
                    ssh -o StrictHostKeyChecking=no -i /var/lib/jenkins/QA.pem ${REMOTE_USER}@${REMOTE_HOST} "
                        pkill -f ${APP_JAR_NAME} || true
                        nohup java -jar ${REMOTE_PATH}/${APP_JAR_NAME} > app.log 2>&1 &
                    "
>>>>>>> 8aabe0ab384b7f07c49a61bff948ef2e44aeb7af
                """
            }
        }
    }
}

