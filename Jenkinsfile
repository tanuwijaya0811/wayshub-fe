def branch = "main"
def remote = "origin"
def directory = "~/jenkins/wayshub-fe"
def server = "tanu96@103.150.116.19"
def cred = "batch24ssh"

pipeline {
    agent any

    stages {
        stage('repo pull') {
            steps {
                sshagent([cred]) {
                    sh """
                        ssh -o StrictHostKeyChecking=no ${server} << EOF
                        cd ${directory}
                        git pull ${remote} ${branch}
                        exit
                        EOF
                    """
                }
            }
        }

        stage('docker build') {
            steps {
                sshagent([cred]) {
                    sh """
                        ssh -o StrictHostKeyChecking=no ${server} << EOF
                        cd ${directory}
                        docker build -t wayshub-fe .
                        exit
                        EOF
                    """
                }
            }
        }

        stage('docker run') {
            steps {
                sshagent([cred]) {
                    sh """
                        ssh -o StrictHostKeyChecking=no ${server} << EOF
                        docker run -d -p 3001:3000 --tty --name frontend wayshub-fe
                        exit
                        EOF
                    """
                }
            }
        }
    }
}
