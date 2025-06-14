

pipeline {
    agent any

    environment {
        AWS_REGION = 'us-east-1'
        IMAGE_NAME = 'sample-ecs-app'
        ECR_REPO = '123456789012.dkr.ecr.us-east-1.amazonaws.com/sample-ecs-app'
        CLUSTER_NAME = 'sample-cluster'
        SERVICE_NAME = 'sample-service'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/<your-username>/sample-ecs-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Push Image to ECR') {
            steps {
                sh '''
                aws ecr get-login-password --region $AWS_REGION | \
                docker login --username AWS --password-stdin $ECR_REPO

                docker tag $IMAGE_NAME:latest $ECR_REPO:latest
                docker push $ECR_REPO:latest
                '''
            }
        }

        stage('Deploy to ECS') {
            steps {
                sh '''
                aws ecs update-service \
                  --cluster $CLUSTER_NAME \
                  --service $SERVICE_NAME \
                  --force-new-deployment \
                  --region $AWS_REGION
                '''
            }
        }
    }
}

