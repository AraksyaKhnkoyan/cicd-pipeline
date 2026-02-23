pipeline {
    agent any

    environment {
        NODEJS_HOME = tool name: 'node', type: 'NodeJS'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'npm install'
                sh 'npm run build'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
        }

        stage('Change Logo & Set Port') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'main') {
            
                        env.PORT = '3000'
                    } else if (env.BRANCH_NAME == 'dev') {

                        env.PORT = '3001'
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t myapp:${env.BRANCH_NAME} ."
            }
        }

        stage('Deploy') {
            steps {
                sh "docker run -d -p ${env.PORT}:${env.PORT} myapp:${env.BRANCH_NAME}"
            }
        }
    }
}
