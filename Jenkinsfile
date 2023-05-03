pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'node-todo-app'
        EC2_HOST = 'ec2-16-16-143-151.eu-north-1.compute.amazonaws.com'
        EC2_USER = 'ubuntu'
    }
    stages {
        stage('Build') {
            steps {
                script {
                    sh "make build-base"
                }
            }
        }
    }
}