pipeline {
    agent any
    
    tools {
           sonarScamner 'SonarScanner'
          }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh '''
                    sonar-scanner \
                    -Dsonar.projectKey=netflix-clone \
                    -Dsonar.projectName=Netflix-Clone \
                    -Dsonar.sources=.
                    '''
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t netflix-app:latest .'
            }
        }

        stage('Run Container Test') {
            steps {
                sh 'docker run --rm netflix-app:latest nginx -t'
            }
        }

        stage('Trivy Image Scan') {
            steps {
                sh 'trivy image --exit-code 0 --severity HIGH,CRITICAL netflix-app:latest'
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

        stage('Deploy to EKS') {
            steps {
                sh '''
                kubectl apply -f k8s/deployment.yaml
                kubectl apply -f k8s/service.yaml
                kubectl rollout restart deployment/netflix-app
                kubectl rollout status deployment/netflix-app
                '''
            }
        }
    }
}
