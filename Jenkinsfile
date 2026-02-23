pipeline {
    agent any

    tools {
        nodejs 'node'   // MUST match the name exactly: node
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
                    } else {
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
                sh """
                docker rm -f myapp-${env.BRANCH_NAME} || true
                docker run -d --name myapp-${env.BRANCH_NAME} -p ${env.PORT}:${env.PORT} myapp:${env.BRANCH_NAME}
                """
            }
        }
    }
}
