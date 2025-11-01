pipeline {
    agent any

    environment {
        AWS_REGION = "ap-south-1"
        STACK_NAME = "SimpleNetworkStack"
        AWS_CREDENTIALS = "aws-cred"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/yourname/network-pipeline.git'
            }
        }

        stage('Deploy CloudFormation Stack') {
            steps {
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
            echo "✅ Network stack deployed successfully!"
        }
        failure {
            echo "❌ Deployment failed."
        }
    }
}
