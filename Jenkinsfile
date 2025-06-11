pipeline {
    agent any

    environment {
        REMOTE_HOST = '43.205.125.55'
        REMOTE_USER = 'ubuntu'
        REMOTE_PATH = '/home/ubuntu'
        APP_JAR_NAME = 'app.jar'
    }

    stages {
        stage('Clone') {
            steps {
                echo 'Cloning the repository...'
                git branch: 'main', url: 'https://github.com/Tony3231/my-ci-cd-app.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the application...'
                sh "mkdir -p target && echo 'This is a dummy app.jar content' > target/${APP_JAR_NAME}"
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying the application to EC2...'
                withCredentials([sshUserPrivateKey(credentialsId: 'deploy-key', keyFileVariable: 'SSH_KEY_PATH')]) {
                    sh """
                        echo "Copying JAR to EC2..."
                        scp -o StrictHostKeyChecking=no -i \$SSH_KEY_PATH target/${APP_JAR_NAME} ${REMOTE_USER}@${REMOTE_HOST}:${REMOTE_PATH}/

                        echo "Restarting app on EC2..."
                        ssh -o StrictHostKeyChecking=no -i \$SSH_KEY_PATH ${REMOTE_USER}@${REMOTE_HOST} << 'EOF'
pkill -f "java -jar ${REMOTE_PATH}/${APP_JAR_NAME}" || true
echo "Old process killed (if any)."

nohup java -jar ${REMOTE_PATH}/${APP_JAR_NAME} > ${REMOTE_PATH}/output.log 2>&1 &
echo "New application started."
EOF
                    """
                }
            }
        }
    }
}
