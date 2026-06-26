pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t netflix-app:latest .'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run --rm netflix-app:latest nginx -t'
            }
        }
    }
}
