pipeline {
    agent any

    environment {
        AWS_REGION = 'us-west-2'
        MONGO_URL  = credentials('mongo-url')
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/kushal1997/MERNwithMicroservices.git'
            }
        }

        stage('Login to ECR') {
            steps {
                withCredentials([
                    string(credentialsId: 'aws-access-key',    variable: 'AWS_ACCESS_KEY_ID'),
                    string(credentialsId: 'aws-secret-key',    variable: 'AWS_SECRET_ACCESS_KEY'),
                    string(credentialsId: 'account-id',        variable: 'ACCOUNT_ID')
                ]) {
                    script {
                        env.ECR_URL = "${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com"
                    }

                    sh '''
                        aws configure set aws_access_key_id "$AWS_ACCESS_KEY_ID"
                        aws configure set aws_secret_access_key "$AWS_SECRET_ACCESS_KEY"
                        aws configure set default.region "$AWS_REGION"

                        aws ecr get-login-password --region "$AWS_REGION" | docker login --username AWS --password-stdin "$ECR_URL"
                    '''
                }
            }
        }
    }
}
