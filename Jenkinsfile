def runCommandOnEC2 = { cmd ->
    sh """
    ssh -o StrictHostKeyChecking=no ${EC2_USER}@${EC2_HOST} '${cmd}'
    """
}

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
        stage('Docker Install') {
            steps {
                sshagent(['dmpodporin-aws']) {
                    script{
                    runCommandOnEC2(
                        """
                        sudo apt-get remove docker docker-engine docker.io containerd runc           
                        sudo apt-get update
                        sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
                        sudo docker run hello-world                   
                        """
                    )
                }
            }
            }
        }
        stage('Docker Install') {
            steps {
                sshagent(['dmpodporin-aws']) {
                    script{
                    runCommandOnEC2(
                        """
                        sudo apt-get remove docker docker-engine docker.io containerd runc           
                        sudo apt-get update
                        sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
                        sudo docker run hello-world               
                        """
                    )
                }
            }
            }
        }




    }
}