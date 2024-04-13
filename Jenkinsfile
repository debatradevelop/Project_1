pipeline {
    agent any
    stages {
        stage('Install Puppet Agent') {
            steps {
                sh 'ssh user@172.31.6.107 "sudo apt install puppet -y"'
            }
        }
        stage('Push Ansible Configuration') {
            steps {
                sh 'ansible-playbook -i "172.31.6.107," playbook.yml'
            }
        }
        stage('Build and Deploy PHP Docker Container') {
            steps {
                git 'https://github.com/debatradevelop/Project_1.git'
                sh 'docker build -t your-php-app /home/edureka/project1/projCert/website/'
                sh 'docker run -d -p 8080:80 your-php-app'
            }
        }
    }
    post {
        failure {
            sh 'ssh user@172.31.6.107 "docker stop $(docker ps -q)"'
        }
    }
}
