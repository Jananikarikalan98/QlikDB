pipeline {
    agent any

    environment {
        REPLICATE_SERVER = "http://your-replicate-server:8080"  // Qlik Replicate server URL
        REPO_BRANCH      = "main"
    }

    stages {
        stage('Checkout Repository') {
            steps {
                echo "Cloning GitHub repository..."
                git branch: "${REPO_BRANCH}", url: 'https://github.com/Jananikarikalan98/QlikDB.git'
            }
        }

        stage('Find Task JSON Files') {
            steps {
                echo "Listing all JSON task files in repo..."
                script {
                    TASK_FILES = sh(script: "ls *.json", returnStdout: true).trim().split("\\n")
                    echo "Found tasks: ${TASK_FILES}"
                }
            }
        }

        stage('Validate and Deploy Tasks') {
            steps {
                script {
                    for (task in TASK_FILES) {
                        echo "Processing task file: ${task}"

                        // Validate JSON
                        sh "jq empty ${task}"

                        // Deploy task
                        withCredentials([usernamePassword(credentialsId: 'bcba415c-74bf-4e40-a055-6307d336488e', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                            sh """
                            curl -X POST -u ${USER}:${PASS} \
                                 -H 'Content-Type: application/json' \
                                 -d @${task} \
                                 ${REPLICATE_SERVER}/replicate/api/v1/tasks
                            """
                        }

                        // Start replication task
                        withCredentials([usernamePassword(credentialsId: 'bcba415c-74bf-4e40-a055-6307d336488e', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                            sh """
                            curl -X POST -u ${USER}:${PASS} \
                                 ${REPLICATE_SERVER}/replicate/api/v1/tasks/start
                            """
                        }
                    }
                }
            }
        }

        stage('Check Replication Status') {
            steps {
                echo "Fetching replication tasks status..."
                withCredentials([usernamePassword(credentialsId: 'bcba415c-74bf-4e40-a055-6307d336488e', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh """
                    curl -X GET -u ${USER}:${PASS} \
                         ${REPLICATE_SERVER}/replicate/api/v1/tasks/status
                    """
                }
            }
        }
    }

    post {
        success {
            echo "All Qlik Replicate tasks deployed successfully."
        }
        failure {
            echo "Qlik Replicate CI/CD pipeline failed. Check logs for details."
        }
    }
}
