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

        stage('Stop Task if Running') {
            steps {
                bat """
                cd "%REPLICATE_BIN%"
                repctl.exe stoptask task=%TASK_NAME% || echo Task already stopped
                """
            }
        }

        stage('Import Full Load Task from GitHub') {
            steps {
                bat """
                cd "%REPLICATE_BIN%"
                repctl.exe import task json_file="%WORKSPACE%\\task2.json"
                """
            }
        }

        stage('Run Full Load Task') {
            steps {
                bat """
                cd "%REPLICATE_BIN%"
                repctl.exe runtask task=%TASK_NAME%
                """
            }
        }
    }

    post {
        success {
            echo "Full Load task2 imported and started successfully"
        }
        failure {
            echo "CI/CD failed – check Replicate logs"
        }
    }
}
