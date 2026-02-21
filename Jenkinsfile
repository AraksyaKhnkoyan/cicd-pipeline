pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: "${params.BRANCH}", url: 'https://github.com/<your-username>/my-cicd-pipeline.git'
            }
        }
        stage('Set Environment') {
            steps {
                script {
                    if (params.BRANCH == 'main') {
                        env.PORT = '3000'
                        sh 'cp logos/main_logo.svg src/logo.svg'
                    } else {
                        env.PORT = '3001'
                        sh 'cp logos/dev_logo.svg src/logo.svg'
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                sh """
                docker stop my-app || true
                docker rm my-app || true
                docker build -t my-app:${params.BRANCH} .
                docker run -d -p ${PORT}:${PORT} --name my-app my-app:${params.BRANCH}
                """
            }
        }
    }
}
