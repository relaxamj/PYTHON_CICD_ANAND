pipeline {
    agent any

    environment {
        AWS_ACCOUNT_ID = "416394563103"
        AWS_REGION = "us-east-1"
        ECR_REPOSITORY = "my-first-image"

        IMAGE_TAG = "${BUILD_NUMBER}"

        ECR_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPOSITORY}"
    }

    stages {

        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Clone Code') {
            steps {
                git branch: 'main',
                url: 'https://github.com/YOUR_GITHUB_USERNAME/YOUR_REPO.git'
            }
        }

        stage('Verify Docker') {
            steps {
                sh 'docker --version'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t $ECR_REPOSITORY:$IMAGE_TAG .
                '''
            }
        }

        stage('Authenticate to AWS ECR') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-ecr-creds'
                ]]) {

                    sh '''
                    aws ecr get-login-password --region $AWS_REGION | \
                    docker login --username AWS --password-stdin \
                    $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com
                    '''
                }
            }
        }

        stage('Tag Docker Image') {
            steps {
                sh '''
                docker tag $ECR_REPOSITORY:$IMAGE_TAG \
                $ECR_URI:$IMAGE_TAG

                docker tag $ECR_REPOSITORY:$IMAGE_TAG \
                $ECR_URI:latest
                '''
            }
        }

        stage('Push Docker Image to ECR') {
            steps {
                sh '''
                docker push $ECR_URI:$IMAGE_TAG

                docker push $ECR_URI:latest
                '''
            }
        }

        stage('Cleanup Local Docker Images') {
            steps {
                sh '''
                docker rmi $ECR_URI:$IMAGE_TAG || true
                docker rmi $ECR_URI:latest || true
                docker rmi $ECR_REPOSITORY:$IMAGE_TAG || true
                '''
            }
        }
    }

    post {
        success {
            echo 'Docker Image Successfully Pushed to AWS ECR'
        }

        failure {
            echo 'Pipeline Failed'
        }
    }
}
