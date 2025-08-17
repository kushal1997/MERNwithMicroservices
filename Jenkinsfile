pipeline{
    agent any

    environment {
        AWS_REGION = 'us-west-2'
        ACCOUNT_ID = credentials('account-id')
        AWS_ACCESS_KEY = credentials('aws-access-key')
        AWS_SECRET_KEY = credentials('aws-secret-key')
        MONGO_URL = credentials('mongo-url')
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/kushal1997/MERNwithMicroservices.git'
            }
        }

        stage('Login to ECR') {
        steps {
            script {
                ECR_URL = "${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com"
            }
            withCredentials([
                string(credentialsId: 'aws-access-key', variable: 'AWS_ACCESS_KEY'),
                string(credentialsId: 'aws-secret-key', variable: 'AWS_SECRET_KEY')
            ]) {
                sh """
                    aws configure set aws_access_key_id $AWS_ACCESS_KEY
                    aws configure set aws_secret_access_key $AWS_SECRET_KEY
                    aws configure set default.region $AWS_REGION

                    aws ecr get-login-password --region $AWS_REGION | \
                    docker login --username AWS --password-stdin ${ECR_URL}
                """
                }
            }
        }
    }
}