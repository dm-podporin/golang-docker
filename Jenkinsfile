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
        stage('Test') {
            steps {
                script {
                    sh "make build-test"
                    sh "make test-unit"
                }
            }
        }
        stage('List of content') {
            steps {
                script {
                    sh "ls"
                }
            }
        }
        stage('Build the final Docker image') {
            steps {
                script {
                    sh "  make build"
                }
            }
        }
    }
}