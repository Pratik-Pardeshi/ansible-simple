pipeline {
    agent any

    stages {
        stage('Pull Files from Git') {
            steps {
                script {
                    sh 'rm -rf /tmp/deployment && git clone https://github.com/Pratik-Pardeshi/ansible-simple.git /tmp/deployment'
                    sh 'ls -l /tmp/deployment/'  // Debugging step
                }
            }
        }

        stage('Ensure Deployment Directory on Ansible Server') {
            steps {
                script {
                    sh """
                    ssh -o StrictHostKeyChecking=no ansible@172.31.34.55 '
                    sudo mkdir -p /home/ansible/deployment &&
                    sudo chown -R ansible:ansible /home/ansible/deployment &&
                    sudo chmod -R 777 /home/ansible/deployment &&
                    ls -ld /home/ansible/deployment/'
                    """
                }
            }
        }

        stage('Copy Files to Ansible Server') {
            steps {
                script {
                    sh """
                    if [ -f /tmp/deployment/index.html ] && [ -f /tmp/deployment/playbook.yml ]; then
                        scp -o StrictHostKeyChecking=no /tmp/deployment/index.html ansible@172.31.34.55:/home/ansible/deployment/
                        scp -o StrictHostKeyChecking=no /tmp/deployment/playbook.yml ansible@172.31.34.55:/home/ansible/deployment/
                    else
                        echo "Error: Files not found!"
                        exit 1
                    fi
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
