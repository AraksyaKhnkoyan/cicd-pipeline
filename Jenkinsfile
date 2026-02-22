pipeline {
    agent any

    environment {
        PORT = ""
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Set Environment') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'main') {
                        env.PORT = "3000"
                        sh 'cp logos/main_logo.svg src/logo.svg'
                    } else if (env.BRANCH_NAME == 'dev') {
                        env.PORT = "3001"
                        sh 'cp logos/dev_logo.svg src/logo.svg'
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t my-app:${env.BRANCH_NAME} ."
            }
        }

        stage('Deploy') {
            steps {
                sh """
                docker stop my-app-${env.BRANCH_NAME} || true
                docker rm my-app-${env.BRANCH_NAME} || true
                docker run -d -p ${PORT}:${PORT} --name my-app-${env.BRANCH_NAME} my-app:${env.BRANCH_NAME}
                """
            }
        }
    }
}
