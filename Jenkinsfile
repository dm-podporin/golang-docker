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
                        sudo apt-get remove docker docker-engine docker.io containerd runc
                        sudo apt-get update
                        sudo apt-get install ca-certificates curl gnupg
                        sudo install -m 0755 -d /etc/apt/keyrings
                        curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
                        sudo chmod a+r /etc/apt/keyrings/docker.gpg
                        echo \
                        "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
                        "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
                        sudo tee /etc/apt/sources.list.d/docker.list > /dev/null              
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