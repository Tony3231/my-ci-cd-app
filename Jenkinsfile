// Jenkinsfile (Groovy)

pipeline {
    agent any // This means the pipeline can run on any available agent

    // Define environment variables for better readability and maintainability.
    // These will be available throughout your pipeline.
    environment {
        REMOTE_HOST = '13.233.238.104' // The IP address of your EC2 instance
        REMOTE_USER = 'ubuntu'         // The username to log in to your EC2 instance
        REMOTE_PATH = '/home/ubuntu'   // The directory on the EC2 instance where the app will be deployed
        APP_JAR_NAME = 'app.jar'       // The name of your application's JAR file
    }

    stages {
        stage('Clone') {
            steps {
                echo 'Cloning the repository...'
                // Clones your GitHub repository.
                // The 'main' branch will be checked out.
                git branch: 'main', url: 'https://github.com/Tony3231/my-ci-cd-app.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the application...'
                // IMPORTANT: Replace this placeholder with your actual build command!
                // This command should compile your Java code and produce a JAR file.
                //
                // Examples:
                // If using Maven:
                // sh 'mvn clean package -DskipTests'
                //
                // If using Gradle:
                // sh './gradlew clean build -x test'
                //
                // For demonstration, a dummy JAR file is created.
                sh "mkdir -p target && echo 'This is a dummy app.jar content' > target/${APP_JAR_NAME}"
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying the application to EC2...'
                // This block securely retrieves your SSH private key from Jenkins Credentials.
                // 'credentialsId: 'deploy-key'' refers to the ID you set in Jenkins Credentials.
                // 'keyFileVariable: 'SSH_KEY_PATH'' creates a temporary file with the key
                // and sets its path in the environment variable 'SSH_KEY_PATH'.
                withCredentials([sshUserPrivateKey(credentialsId: 'deploy-key', keyFileVariable: 'SSH_KEY_PATH')]) {
                    sh """
                        # 1. Copy the built JAR file to the remote EC2 instance.
                        # -i \$SSH_KEY_PATH tells scp to use the private key provided by Jenkins.
                        scp -i \$SSH_KEY_PATH target/${APP_JAR_NAME} ${REMOTE_USER}@${REMOTE_HOST}:${REMOTE_PATH}/

                        # 2. Connect to the EC2 instance and restart the Java application.
                        ssh -i \$SSH_KEY_PATH ${REMOTE_USER}@${REMOTE_HOST} '
                            # Stop any existing running instance of the application.
                            # 'pkill -f' searches for processes by name (java -jar app.jar).
                            # '|| true' ensures the command doesn't fail the pipeline if no process is found.
                            pkill -f "java -jar ${REMOTE_PATH}/${APP_JAR_NAME}" || true
                            echo "Old process killed (if any)."

                            # Start the new Java application in the background.
                            # 'nohup' allows the process to run even after the SSH session disconnects.
                            # 'java -jar' executes your application.
                            # '> output.log 2>&1 &' redirects standard output and error to output.log and runs in background.
                            nohup java -jar ${REMOTE_PATH}/${APP_JAR_NAME} > ${REMOTE_PATH}/output.log 2>&1 &
                            echo "New application started."
                        '
                    """
                }
            }
        }
    }
}
