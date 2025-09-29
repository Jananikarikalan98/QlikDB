pipeline {
    agent any

    environment {
        // Jenkins credentials ID you created
        QLIK_CREDS = credentials('bcba415c-74bf-4e40-a055-6307d336488e')
        QLIK_API = "http://localhost:3562/replicate/api/v1"  // Update if different
    }

    stages {
        stage('Checkout Repo') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Jananikarikalan98/QlikDB.git'
            }
        }

        stage('Find Task JSON Files') {
            steps {
                echo 'Listing all JSON task files in repo...'
                bat 'dir *.json'
            }
        }

        stage('Validate and Deploy Tasks') {
            steps {
                echo 'Deploying task2.json to Qlik Replicate...'
                bat """
                curl -u %QLIK_CREDS_USR%:%QLIK_CREDS_PSW% -X POST ^
                -H "Content-Type: application/json" ^
                -d @task2.json %QLIK_API%/tasks
                """
            }
        }
    }

    post {
        success {
            echo 'Task successfully deployed to Qlik Replicate!'
        }
        failure {
            echo 'Deployment failed. Check logs.'
        }
    }
}
