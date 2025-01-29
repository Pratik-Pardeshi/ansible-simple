pipeline {
    agent any

    environment {
        ANSIBLE_SERVER = '172.31.34.144' // Ansible server address
        PLAYBOOK_PATH = '/home/ansible/playbook.yml' // Path to playbook on Ansible server
        SSH_KEY_PATH = '/var/lib/jenkins/.ssh/id_rsa' // Path to private SSH key for Jenkins
    }

    stages {
        stage('Create Playbook and Run') {
            steps {
                script {
                    // Step 1: SSH into Ansible server and create playbook.yml
                    sh """
                    ssh -i ${SSH_KEY_PATH} ansible@${ANSIBLE_SERVER} 'echo "---" > ${PLAYBOOK_PATH} && echo "- name: Deploy Example Application" >> ${PLAYBOOK_PATH} && echo "  hosts: webserver" >> ${PLAYBOOK_PATH} && echo "  tasks:" >> ${PLAYBOOK_PATH} && echo "    - name: Ensure Apache is installed" >> ${PLAYBOOK_PATH} && echo "      ansible.builtin.yum:" >> ${PLAYBOOK_PATH} && echo "        name: httpd" >> ${PLAYBOOK_PATH} && echo "        state: present" >> ${PLAYBOOK_PATH} && echo "    - name: Start Apache service" >> ${PLAYBOOK_PATH} && echo "      ansible.builtin.service:" >> ${PLAYBOOK_PATH} && echo "        name: httpd" >> ${PLAYBOOK_PATH} && echo "        state: started" >> ${PLAYBOOK_PATH} && echo "        enabled: yes" >> ${PLAYBOOK_PATH}'
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
