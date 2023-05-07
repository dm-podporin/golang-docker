pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'golang-app'
        EC2_HOST = 'ec2-13-50-242-196.eu-north-1.compute.amazonaws.com'
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
                    script{
                    def runCommandOnEC2 = { cmd ->
                        sh """
                        ssh -o StrictHostKeyChecking=no ${EC2_USER}@${EC2_HOST} '${cmd}'
                        """
                    }

                    runCommandOnEC2(
                        """
                        sudo apt-get update
                        sudo apt-get -y install apt-transport-https ca-certificates curlgnupg-agent software-properties-common
                        curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
                        sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
                        sudo apt-get install -y docker-ce=5:18.09.5~3-0~ubuntu-bionic docker-ce-cli=5:18.09.5~3-0~ubuntu-bionic containerd.io
                        sudo docker version
                        """
                    )
                }
            }
            }
        }   
    }
}