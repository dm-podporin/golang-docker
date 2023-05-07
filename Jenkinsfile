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
                        sudo apt-get -y update
                        sudo apt install -y apt-transport-https ca-certificates curl software-properties-common
                        curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
                        sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
                        apt-cache policy docker-ce
                        sudo apt install -y docker-ce
                        sudo systemctl status docker
                        sudo systemctl enable docker
                        sudo docker ps
                        sudo apt install -y make
                        """
                    )
                }
            }
            }
        }   
    }
}