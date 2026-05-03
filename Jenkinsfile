pipeline {
    agent any

    environment {
        IMAGE_NAME = "sample-node-app"
        IMAGE_TAG = "v1.%BUILD_NUMBER%"
        CONTAINER_NAME = "sample-node-container"
        PORT = "3000"
    }

    stages {

        stage('Clone Code') {
            steps {
                git 'https://github.com/SK03Sandeep/sample-node-project.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'npm test'
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
                docker rm -f %CONTAINER_NAME% || exit 0
                docker run -d -p %PORT%:3000 --name %CONTAINER_NAME% %IMAGE_NAME%:%IMAGE_TAG%
                '''
            }
        }
    }
}
