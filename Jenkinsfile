pipeline {
    agent any
    stages {
        stage('Install Puppet Agent') {
            steps {
                script {
                    // Start an SSH agent
                    sshagent(credentials: ['debatra_rsa']) {
                        // Run the SSH command on the master server
                        sh 'sudo apt install puppet -y'
                    }
                }
            }
        }
        stage('Push Ansible Configuration') {
            steps {
                sh 'ansible-playbook -i "localhost," playbook.yml'
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
            // Perform actions on the master server in case of failure
            sh 'docker stop $(docker ps -q)'
        }
    }
}
