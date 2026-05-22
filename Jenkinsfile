pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/relaxamj/PYTHON_CICD_ANAND.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t relaxamj/python-app:latest .'
            }
        }

        stage('Build Image') {
            steps {
                sh 'docker build -t python-app:latest .'
            }
        }

        stage('Tag Image') {
            steps {
                sh 'docker tag python-app:latest relaxamj/python-app:latest'
            }
        }

        stage('Push Image') {
            steps {
                withDockerRegistry([url: 'https://index.docker.io/v1/', credentialsId: 'dockerhub-creds']) {
                    sh 'docker push relaxamj/python-app:latest'
                }
            }
        }
    }
}
