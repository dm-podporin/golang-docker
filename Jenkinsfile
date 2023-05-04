pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'golang-app'
        DOCKER_TAG = 
        EC2_HOST = 'ec2-16-170-37-245.eu-north-1.compute.amazonaws.com'
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
        // stage('Test') {
        //     steps {
        //         script {
        //             sh "make build-test"
        //             sh "make test-unit"
        //         }
        //     }
        // }
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
        stage('Deploy to EC2') {
            steps {
                   sshagent(['dmpodporin-aws']) {
                    sh """
                        ssh -o StrictHostKeyChecking=no ${EC2_USER}@${EC2_HOST} 'docker-compose down && docker-compose pull && docker-compose up -d'
                """
            }
            }
        }   
    }
}