pipeline {
    agent any

    stages {

        stage('Checkout GitHub Repo') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Jananikarikalan98/QlikDB.git'
            }
        }

        stage('List Files') {
            steps {
                bat 'dir'
            }
        }

        stage('Verify task1.json') {
            steps {
                bat 'type task1.json'
            }
        }
    }
}

