pipeline {
    agent any

    environment {
        REMOTE_HOST = '13.204.53.183'
        REMOTE_USER = 'ubuntu'
        REMOTE_PATH = '/home/ubuntu'
        APP_JAR_NAME = 'app/build/libs/app.jar'
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
                echo '🔧 Building the application...'
                sh './gradlew build'
            }
        }

        stage('Deploy') {
            steps {
                echo '🚀 Deploying to EC2 instance...'
                sh """
                    chmod 400 /var/lib/jenkins/QA.pem
                    scp -o StrictHostKeyChecking=no -i /var/lib/jenkins/QA.pem build/libs/${APP_JAR_NAME} ${REMOTE_USER}@${REMOTE_HOST}:${REMOTE_PATH}/
                    ssh -o StrictHostKeyChecking=no -i /var/lib/jenkins/QA.pem ${REMOTE_USER}@${REMOTE_HOST} "
                      pkill -f ${APP_JAR_NAME} || true
                      nohup java -jar ${REMOTE_PATH}/${APP_JAR_NAME} > app.log 2>&1 &
                    "
                """
            }
        }
    }
}
