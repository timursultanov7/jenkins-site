pipeline {
    agent any

    environment {
        NODE_PATH = "/Users/timursultanov/.nvm/versions/node/v16.14.2/bin"
        PATH = "${env.NODE_PATH}:${env.PATH}"
    }

    stages {
        stage('Start Server') {
            steps {
                dir('jenkins-site') {
                    script {
                        sh 'pwd'
                        sh 'npm install'
                        sh 'npm start & echo $! > .pid'
                        env.NEW_SERVER_PID = sh(script: "cat .pid", returnStdout: true).trim()
                        sleep 5
                    }
                }
            }
        }

        stage('Run Tests') {
            steps {
                dir('jenkins-site') {
                    script {
                        sh 'npm test'
                    }
                }
            }
        }
    }

    post {
        always {
            dir('jenkins-site') {
                script {
                    sh 'echo "Stopping server with PID: ${NEW_SERVER_PID}"'
                    sh 'kill ${NEW_SERVER_PID} || echo "Process not found"'
                }
            }
        }
    }
}
