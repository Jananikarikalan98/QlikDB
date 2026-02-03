pipeline {
    agent any

    environment {
        REPLICATE_BIN = "C:\\Program Files\\Attunity\\Replicate\\bin"
        TASK_NAME = "task2"
    }

    stages {

        stage('Checkout Repo') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Jananikarikalan98/QlikDB.git'
            }
        }

        stage('Verify Task JSON Exists') {
            steps {
                bat 'dir task2.json'
            }
        }

        stage('Stop Full Load Task (if running)') {
            steps {
                bat """
                cd "%REPLICATE_BIN%"
                repctl.exe stoptask task=%TASK_NAME% || echo Task already stopped
                """
            }
        }
    }

    post {
        success {
            echo "CI/CD completed: task2 safely stopped and version-controlled"
        }
        failure {
            echo "CI/CD failed – check Jenkins logs"
        }
    }
}
