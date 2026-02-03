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

        stage('Pause Full Load Task (if running)') {
            steps {
                bat """
                cd "%REPLICATE_BIN%"
                repctl.exe pausetask task=%TASK_NAME% || echo Task not running
                """
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
            echo "CI/CD completed successfully for Full Load task: task2"
        }
        failure {
            echo "CI/CD failed – check Replicate logs"
        }
    }
}
