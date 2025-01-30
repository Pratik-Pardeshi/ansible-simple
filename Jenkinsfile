pipeline {
    agent any

    stages {
        stage('Clone Repo on Ansible Server') {
            steps {
                script {
                    sh '''
                    ssh -o StrictHostKeyChecking=no ansible@172.31.34.144 "
                    sudo rm -rf /home/ansible/deployment &&
                    git clone https://github.com/Pratik-Pardeshi/ansible-simple.git /home/ansible/deployment &&
                    ls -l /home/ansible/deployment"
                    '''
                }
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                script {
                    sh '''
                    ssh -o StrictHostKeyChecking=no ansible@172.31.34.144 "
                    ansible-playbook /home/ansible/deployment/playbook.yml
                    "
                    '''
                }
            }
        }
    }
}
