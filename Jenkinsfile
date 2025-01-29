pipeline {
    agent any

    environment {
        ANSIBLE_SERVER = '172.31.14.31' // Ansible server address
        PLAYBOOK_PATH = '/home/ansible/playbook.yml' // Path to playbook on Ansible server
        SSH_KEY_PATH = '/var/lib/jenkins/.ssh/id_rsa' // Path to private SSH key for Jenkins
    }

    stages {
        stage('Create Playbook and Run') {
            steps {
                script {
                    // Step 1: SSH into Ansible server and create playbook.yml
                    sh """
                    ssh -i ${SSH_KEY_PATH} ansible@${ANSIBLE_SERVER} << 'EOF'
                    # Create playbook.yml on Ansible server
                    cat <<EOL > ${PLAYBOOK_PATH}
                    ---
                    - name: Deploy Example Application
                      hosts: localhost
                      tasks:
                        - name: Ensure Apache is installed
                          ansible.builtin.yum:
                            name: httpd
                            state: present
                        - name: Start Apache service
                          ansible.builtin.service:
                            name: httpd
                            state: started
                            enabled: yes
                    EOL
                    EOF
                    """

                    // Step 2: Run the playbook
                    sh """
                    ssh -i ${SSH_KEY_PATH} ansible@${ANSIBLE_SERVER} 'ansible-playbook ${PLAYBOOK_PATH}'
                    """
                }
            }
        }
    }

    post {
        always {
            echo "Playbook execution completed"
        }
    }
}
