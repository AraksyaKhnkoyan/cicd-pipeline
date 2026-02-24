pipeline {
    agent any

    tools {
        nodejs 'node'   
    }

    environment {
        IMAGE_NAME = "myapp"
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

        stage('Set Port') {
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
                sh "docker build -t ${IMAGE_NAME}:${env.BRANCH_NAME} ."
            }
        }

        stage('Deploy') {
            steps {
                sh """
                docker rm -f ${IMAGE_NAME}-${env.BRANCH_NAME} || true
                docker run -d \
                  --name ${IMAGE_NAME}-${env.BRANCH_NAME} \
                  -p ${env.PORT}:3000 \
                  ${IMAGE_NAME}:${env.BRANCH_NAME}
                """
            }
        }
    }
}
