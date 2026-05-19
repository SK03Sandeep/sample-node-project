pipeline {
    agent any

    options {
        skipDefaultCheckout(true)
    }

    environment {
        IMAGE_NAME = "sample-node-app"
        IMAGE_TAG = "v1.${BUILD_NUMBER}"
        CONTAINER_NAME = "sample-node-container"
        PORT = "3000"
    }

    stages {

        stage('Clone Code') {
            steps {
                    url: 'https://github.com/SK03Sandeep/sample-node-project.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
                bat 'npm audit fix'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t %IMAGE_NAME%:%IMAGE_TAG% .'
            }
        }

        stage('Deploy Container') {
            steps {
                bat '''
                docker rm -f %CONTAINER_NAME%
                docker run -d -p %PORT%:3000 --name %CONTAINER_NAME% %IMAGE_NAME%:%IMAGE_TAG%
                '''
            }
        }
    }

    post {
        success {
            archiveArtifacts artifacts: '**/package*.json,Dockerfile',
                             allowEmptyArchive: true

            cleanWs()
        }
    }
}
