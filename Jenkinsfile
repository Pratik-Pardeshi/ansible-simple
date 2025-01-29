pipeline {
    agent any

    environment {
        ANSIBLE_SERVER = '172.31.14.31' // Ansible server address
        PLAYBOOK_PATH = '/home/ansible/playbook.yml' // Path to playbook on Ansible server
        SSH_KEY_PATH = '/var/lib/jenkins/.ssh/id_rsa' // Path to private SSH key for Jenkins
    }

    stages {
        stage('SSH to Ansible Server and Run Playbook') {
            steps {
                script {
                    // Run SSH command to execute Ansible playbook
                    sh """
                    ssh -i ${SSH_KEY_PATH} ansible@${ANSIBLE_SERVER} 'ansible-playbook ${PLAYBOOK_PATH}'
                    """
                }
            }
        }
    }

    post {
        always {
            // Clean up any resources or notifications
            echo "Playbook execution completed"
        }
    }
}
