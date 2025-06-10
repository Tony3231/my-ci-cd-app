pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/Tony3231/my-ci-cd-app.git'
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
        sh """
            scp -i your-key.pem target/app.jar ubuntu@13.233.238.104:/home/ubuntu/
            ssh -i your-key.pem ubuntu@13.233.238.104 'nohup java -jar /home/ubuntu/app.jar > output.log 2>&1 &'
        """
    }
}

    }
}
