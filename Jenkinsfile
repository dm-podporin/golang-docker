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

        stage('Docker Instal') {
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
                    )}
                }
            }
        }
        stage('Copy application files to the EC2 instance') {
            steps {
                sshagent(['dmpodporin-aws']) {
                    sh """
                    scp -o StrictHostKeyChecking=no -r ${WORKSPACE}/  ${EC2_USER}@${EC2_HOST}:/home/ubuntu/golang-app/go/
                    """
                }
            }
        }
        stage('Build and run Docker on EC2 instance') {
            steps {
                sshagent(['dmpodporin-aws']) {
                    script{
                    runCommandOnEC2(
                        """
                        cd /home/ubuntu/golang-app/go/ && sudo make build-base && sudo make build && sudo make run
                        """
                    )}
                }
            }
        }
    }
}