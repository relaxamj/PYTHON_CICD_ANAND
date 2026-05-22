pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'dev2', url: 'https://github.com/relaxamj/PYTHON_CICD_ANAND.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t relaxamj/dev2-app:latest .'
            }
        }

        stage('Build Image') {
            steps {
                sh 'docker build -t python-pythondev2:latest .'
            }
        }

        stage('Tag Image') {
            steps {
                sh 'docker tag python-app:latest relaxamj/dev2-app:latest'
            }
        }

        stage('Push Image') {
            steps {
                withDockerRegistry([url: 'https://index.docker.io/v1/', credentialsId: 'dockerhub-creds']) {
                    sh 'docker push relaxamj/dev2-app:latest'
                }
            }
        }
    }
}
