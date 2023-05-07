pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'golang-app'
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
                        ssh -o StrictHostKeyChecking=no ${EC2_USER}@${EC2_HOST} 'sudo apt-get -y update'
                        ssh -o StrictHostKeyChecking=no ${EC2_USER}@${EC2_HOST} 'sudo apt install -y apt-transport-https ca-certificates curl software-properties-common'
                """
            }
            }
        }   
    }
}