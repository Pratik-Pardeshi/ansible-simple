pipeline {
    agent any

    environment {
        SSH_KEY_PATH = '/var/lib/jenkins/.ssh/id_rsa'
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo 'Cloning application repository...'
                git branch: 'main', url: 'https://github.com/Pratik-Pardeshi/new-ansible.git'
            }
        }

        stage('Set up SSH Access') {
            steps {
                echo 'Setting up SSH access...'

                // Ensure the SSH key exists
                sh '''
                if [ ! -f $SSH_KEY_PATH ]; then
                    echo "SSH private key not found. Please configure the SSH key manually."
                    exit 1
                fi
                '''

                // Add SSH key to the SSH agent
                sh '''
                eval $(ssh-agent)
                ssh-add $SSH_KEY_PATH
                ssh-keyscan -H 172.31.24.125 >> ~/.ssh/known_hosts
                '''
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                echo 'Running Ansible playbook...'
                sh '''
                ansible-playbook -i /home/jenkins/inventory.ini /home/jenkins/deploy.yml
                '''
            }
        }
    }
}
