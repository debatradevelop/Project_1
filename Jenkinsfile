pipeline {
    agent any
    stages {
        stage('Install Puppet Agent') {
            steps {
                script {
                    withCredentials([sshUserPrivateKey(credentialsId: 'debatra_rsa', keyFileVariable: 'SSH_KEY')]) {
                        sh 'ssh -i $SSH_KEY edureka@172.31.6.107 "sudo apt install puppet -y"'
                    }
                }
            }
        }
        stage('Push Ansible Configuration') {
            steps {
                script {
                    withCredentials([sshUserPrivateKey(credentialsId: 'debatra_rsa', keyFileVariable: 'SSH_KEY')]) {
                        sh 'ssh -i $SSH_KEY edureka@172.31.6.107 "ansible-playbook -i \"172.31.6.107,\" playbook.yml"'
                    }
                }
            }
        }
        stage('Build and Deploy PHP Docker Container') {
            steps {
                script {
                    withCredentials([sshUserPrivateKey(credentialsId: 'debatra_rsa', keyFileVariable: 'SSH_KEY')]) {
                        git 'https://github.com/debatradevelop/Project_1.git'
                        sh 'ssh -i $SSH_KEY edureka@172.31.6.107 "docker build -t your-php-app /home/edureka/project1/projCert/website/"'
                        sh 'ssh -i $SSH_KEY edureka@172.31.6.107 "docker run -d -p 8080:80 your-php-app"'
                    }
                }
            }
        }
    }
    post {
        failure {
            script {
                withCredentials([sshUserPrivateKey(credentialsId: 'debatra_rsa', keyFileVariable: 'SSH_KEY')]) {
                    sh 'ssh -i $SSH_KEY edureka@172.31.6.107 "docker stop $(docker ps -q)"'
                }
            }
        }
    }
}
