pipeline {
    agent any

    environment {
        AWS_REGION = "us-east-1"                 
        STACK_NAME = "SimpleNetworkStack"
        AWS_CREDENTIALS = "aws-cred"
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo "📦 Checking out code from GitHub..."
                git branch: 'main', url: 'https://github.com/Sanjana1455/network-pipeline.git'
            }
        }

        stage('Deploy CloudFormation Stack') {
            steps {
                echo "🚀 Deploying CloudFormation stack in region: ${AWS_REGION}"
                withAWS(region: "${AWS_REGION}", credentials: "${AWS_CREDENTIALS}") {
                    cfnUpdate(
                        stack: "${STACK_NAME}",
                        file: 'network-template.yaml',
                        create: true,
                        pollInterval: 1000,
                        params: [],
                        timeoutInMinutes: 10
                    )
                }
            }
        }
    }

    post {
        success {
            echo "✅ Network stack deployed successfully in region ${AWS_REGION}!"
        }
        failure {
            echo "❌ Deployment failed. Check CloudFormation events for details."
        }
    }
}
