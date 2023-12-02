pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'ap-southeast-2'
        ECR_REPO_NAME = 'aaronrepo'
    }

    stages {
        stage('Build and Push to ECR') {
            steps {
                script {
                    // Build Docker image
                    def dockerImage = docker.build("${ECR_REPO_NAME}:${env.BUILD_NUMBER}")

                    // Log in to ECR
                    withAWS(credentials: 'AARON', region: 'ap-southeast-2') {
                        docker.withRegistry("https://${AWS_DEFAULT_REGION}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com", 'ecr:latest') {
                            // Push Docker image to ECR
                            dockerImage.push()
                        }
                    }
                }
            }
        }
    }
}
