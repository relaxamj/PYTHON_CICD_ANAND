pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'dev', url: 'https://github.com/relaxamj/PYTHON_CICD_ANAND.git'
            }
        }

        stage('Build Image') {
            steps {
                sh 'docker build -t dev-app:latest .'
            }
        }

        stage('Tag Image') {
            steps {
                sh 'docker tag dev-app:latest relaxamj/python-app:latest'
            }
        }

        stage('Push Image') {
            steps {
                withDockerRegistry([url: 'https://index.docker.io/v1/', credentialsId: 'dockerhub-creds']) {
                    sh 'docker push relaxamj/dev-app:latest'
                }
            }
        }
    }
}
