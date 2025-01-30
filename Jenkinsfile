pipeline {
    agent any
    
    stages {
        stage('Pull Files from Git') {
            steps {
                script {
                    sh 'git clone https://your-git-repo-url.git /tmp/deployment'
                }
            }
        }

        stage('Set Permissions and Copy Files to Ansible Server') {
            steps {
                script {
                    sh """
                    chmod -R 777 /tmp/deployment
                    ssh -o StrictHostKeyChecking=no ansible@172.31.34.144 'sudo mkdir -p /opt/deployment && sudo chmod -R 777 /opt/deployment'
                    scp -o StrictHostKeyChecking=no -r /tmp/deployment/* ansible@172.31.34.144:/opt/deployment/
                    ssh -o StrictHostKeyChecking=no ansible@172.31.34.144 'sudo chmod -R 777 /opt/deployment'
                    """
                }
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                script {
                    sh "ssh -o StrictHostKeyChecking=no ansible@172.31.34.144 'sudo ansible-playbook /opt/deployment/playbook.yml'"
                }
            }
        }
    }
}
