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
stage('Login to ECR') {
    steps {
        sh 'aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 300034476295.dkr.ecr.ap-south-1.amazonaws.com'
    }
}

stage('Tag Image') {
    steps {
        sh 'docker tag netflix-app:latest 300034476295.dkr.ecr.ap-south-1.amazonaws.com/netflix-app:latest'
    }
}

stage('Push to ECR') {
    steps {
        sh 'docker push 300034476295.dkr.ecr.ap-south-1.amazonaws.com/netflix-app:latest'
    }
}
