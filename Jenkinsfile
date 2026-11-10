pipeline {
    agent any
    environment {
        AWS_ACCOUNT_ID = '123456789012' // Replace with your real AWS Account ID
        AWS_DEFAULT_REGION = 'us-east-1' // Replace with your AWS region
        IMAGE_TAG = "build-${BUILD_NUMBER}"
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build Images') {
            steps {
                sh "docker build -t ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}://{IMAGE_TAG} ./user-service"
                sh "docker build -t ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}://{IMAGE_TAG} ./product-service"
                sh "docker build -t ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}://{IMAGE_TAG} ./order-service"
            }
        }
        stage('Push to Amazon ECR') {
            steps {
                // Authenticates securely to ECR using active AWS keys or attached IAM roles
                sh "aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com"
                sh "docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}://{IMAGE_TAG}"
                sh "docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}://{IMAGE_TAG}"
                sh "docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}://{IMAGE_TAG}"
            }
        }
        stage('Deploy to EC2') {
            steps {
                // Replace with your deployment mechanism (like SSH-agent block or AWS Systems Manager)
                // Triggers: docker compose pull && docker compose up -d
                echo "Deploying version ${IMAGE_TAG} straight to target EC2 Instance..."
            }
        }
    }
}
