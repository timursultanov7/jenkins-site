pipeline {
    agent any

    environment {
        NODE_PATH = "/Users/timursultanov/.nvm/versions/node/v16.14.2/bin"
        PATH = "${env.NODE_PATH}:${env.PATH}"
    }

    stages {

      

        stage('Start Server') {
            steps {
                dir('/Users/timursultanov7/.jenkins/workspaces/jenkins-site')
                
                
                 {
                    script {
                        // Запустите новый сервер в фоновом режиме на порту 8082
                        sh 'npm start & echo $! > .pid'
                        
                        // Сохраните PID нового сервера, чтобы можно было остановить его после тестов
                        script {
                            env.NEW_SERVER_PID = sh(script: "cat .pid", returnStdout: true).trim()
                        }

                        // Дайте серверу время на запуск
                        sleep 5
                    }
                }
            }
        }

        stage('Run Tests') {
            steps {
                dir('/Users/timursultanov7/.jenkins//workspaces/jenkins-site') {
                    script {
                        // Запустите тесты
                        sh 'npm test'
                    }
                }
            }
        }
    }

    post {
        always {
            script {
                // Остановите новый сервер
                sh "kill ${env.NEW_SERVER_PID}"
            }
        }
    }
}
