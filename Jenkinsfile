pipeline {
    agent any

    tools {
        nodejs 'NodeJS'
    }

    environment {
        DOCKERHUB_CREDENTIALS = credentials('docker-cred')
        IMAGE_NAME = "${DOCKERHUB_CREDENTIALS_USR}/nodeimage"
    }

    stages {

        stage('Checkout Github') {
            steps {
                git credentialsId: 'jen-doc-git', url: 'https://github.com/HtetAungShine6/node-test-app.git'
            }
        }

        stage('Install System Dependencies') {
            steps {
                sh 'apt-get update && apt-get install -y libatomic1'
            }
        }

        stage('Install Node Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test Code') {
            steps {
                sh 'npm test'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} ."
            }
        }
        stage('Push Docker Image') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                sh "docker push ${IMAGE_NAME}:${BUILD_NUMBER}"
            }
        }
    }

    post {
        success {
            echo 'Build successful!'
        }
        failure {
            echo 'Build failed. Please check the logs for details.'
        }
    }
}