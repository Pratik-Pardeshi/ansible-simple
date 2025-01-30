pipeline {
    agent any

    stages {
        stage('Pull Files from Git') {
            steps {
                script {
                    sh 'rm -rf /tmp/deployment && git clone https://github.com/Pratik-Pardeshi/ansible-simple.git /tmp/deployment'
                }
            }
        }

        stage('Ensure Deployment Directory on Ansible Server') {
            steps {
                script {
                    sh """
                    ssh -o StrictHostKeyChecking=no ansible@172.31.34.55 'mkdir -p /home/ansible/deployment && chmod -R 777 /home/ansible/deployment'
                    """
                }
            }
        }

        stage('Copy Files to Ansible Server') {
            steps {
                script {
                    sh """
                    scp -o StrictHostKeyChecking=no /tmp/deployment/index.html ansible@172.31.34.55:/home/ansible/deployment/
                    scp -o StrictHostKeyChecking=no /tmp/deployment/playbook.yml ansible@172.31.34.55:/home/ansible/deployment/
                    """
                }
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                script {
                    sh "ssh -o StrictHostKeyChecking=no ansible@172.31.34.55 'ansible-playbook /home/ansible/deployment/playbook.yml'"
                }
            }
        }
    }
}
