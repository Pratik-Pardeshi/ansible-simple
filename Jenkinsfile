pipeline {
    agent any

    environment {
        ANSIBLE_PLAYBOOK = '/home/jenkins/deploy.yml' // Path to deploy.yml
        INVENTORY_FILE = '/home/jenkins/inventory.ini' // Path to inventory file
    }

    stages {
        stage('Preparation') {
            steps {
                echo 'Preparing workspace...'
                
                // Ensure required files exist
                sh '''
                if [ ! -f "$ANSIBLE_PLAYBOOK" ]; then
                    echo "Error: Ansible playbook $ANSIBLE_PLAYBOOK not found!"
                    exit 1
                fi

                if [ ! -f "$INVENTORY_FILE" ]; then
                    echo "Error: Inventory file $INVENTORY_FILE not found!"
                    exit 1
                fi
                '''
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                echo 'Running Ansible playbook...'

                // Execute Ansible playbook
                sh '''
                ansible-playbook -i $INVENTORY_FILE $ANSIBLE_PLAYBOOK
                '''
            }
        }
    }

    post {
        success {
            echo 'Ansible playbook executed successfully!'
        }
        failure {
            echo 'Ansible playbook execution failed.'
        }
    }
}
