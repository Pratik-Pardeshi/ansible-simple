pipeline {
    agent any

    environment {
        ANSIBLE_PLAYBOOK = '/home/jenkins/deploy.yml' // Path to deploy.yml
        TARGET_HOST = '172.31.24.125' // Replace with the IP of your web server
        SSH_USER = 'ansible' // Replace with the SSH username for your web server
        SSH_KEY = '/home/jenkins/.ssh/id_rsa' // Path to the private SSH key
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

                if [ ! -f "$SSH_KEY" ]; then
                    echo "Error: SSH private key $SSH_KEY not found!"
                    exit 1
                fi
                '''
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                echo 'Running Ansible playbook...'

                // Execute Ansible playbook without inventory.ini
                sh '''
                ansible-playbook -i "$TARGET_HOST," -u "$SSH_USER" --private-key "$SSH_KEY" "$ANSIBLE_PLAYBOOK"
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
