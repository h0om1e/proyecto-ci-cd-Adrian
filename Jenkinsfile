pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "adridelagado/python-app:latest"
    }

    stages {
        stage('clone') {
            steps {
                checkout scm
            }
        }

        stage('test') {
            steps {
                sh 'python3 -m pip install --break-system-packages flask pytest'
                sh 'python3 -m pytest'
            }
        }

        stage('build image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('dockerhub') {
            steps {
                sh 'docker push $DOCKER_IMAGE'
            }
        }

        stage('deploy') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
            }
        }
    }
}
