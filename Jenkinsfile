pipeline {
    agent any
    stages {
        stage('Install Puppet Agent') {
            steps {
                script {
                    sshagent(credentials: ['debatra_rsa']) {
                        sh 'ssh -i /var/lib/jenkins/.ssh/debatra_rsa edureka@172.31.6.107 "sudo apt install puppet -y"'
                    }
                }
            }
        }
        stage('Push Ansible Configuration') {
            steps {
                script {
                    sshagent(credentials: ['debatra_rsa']) {
                        sh 'ansible-playbook -i "172.31.6.107," playbook.yml'
                    }
                }
            }
        }
        stage('Build and Deploy PHP Docker Container') {
            steps {
                git 'https://github.com/debatradevelop/Project_1.git'
                script {
                    sshagent(credentials: ['debatra_rsa']) {
                        sh 'docker build -t your-php-app /home/edureka/project1/projCert/website/'
                        sh 'docker run -d -p 8080:80 your-php-app'
                    }
                }
            }
        }
    }
    post {
        failure {
            script {
                sshagent(credentials: ['debatra_rsa']) {
                    sh 'ssh -i /var/lib/jenkins/.ssh/debatra_rsa edureka@172.31.6.107 "docker stop $(docker ps -q)"'
                }
            }
        }
    }
}
