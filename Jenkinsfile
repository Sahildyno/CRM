pipeline {
    agent any
    
    environment {
        AWS_REGION = 'ap-south-1'
        ECR_REPO = '686255956220.dkr.ecr.ap-south-1.amazonaws.com/my-app-repo'
        IMAGE_TAG = "latest"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'develop', url:'https://github.com/Sahildyno/CRM.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                
                sh 'docker build -t $my-app-repo:$latest .'
            }
        }
        stage('Push to AWS ECR') {
            steps {
                withAWS(region: "$AWS_REGION", credentials: 'aws-jenkins') {
                    sh 'aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin $ECR_REPO'
                    sh 'docker tag $ECR_REPO:$IMAGE_TAG $ECR_REPO:$IMAGE_TAG'
                    sh 'docker push $ECR_REPO:$IMAGE_TAG'
                }
            }
        }
        stage('Deploy to EC2') {
            steps {
                sshagent(['ec2-key']) {
                    sh 'ssh -o StrictHostKeyChecking=no ubuntu@13.127.4.159 "docker pull $ECR_REPO:$IMAGE_TAG && docker run -d -p 80:80 $ECR_REPO:$IMAGE_TAG"'
                }
            }
        }
    }
}
