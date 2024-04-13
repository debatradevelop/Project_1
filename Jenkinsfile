pipeline {
    agent any
    stages {
        stage('Install Puppet Agent') {
            steps {
                script {
                    withCredentials([sshUserPrivateKey(credentialsId: 'debatra_rsa', keyFileVariable: 'SSH_PRIVATE_KEY')]) {
                        sh '''
                            mkdir -p ~/.ssh
                            echo "$SSH_PRIVATE_KEY" > ~/.ssh/id_rsa
                            chmod 600 ~/.ssh/id_rsa
                            # Install puppet without sudo password prompt
                            echo "your_sudo_password" | sudo -S apt install puppet -y
                        '''
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
