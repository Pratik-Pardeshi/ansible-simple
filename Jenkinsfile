pipeline {
    agent any

    environment {
        INVENTORY_FILE = '/home/jenkins/inventory.ini'
        PLAYBOOK_FILE = '/home/jenkins/deploy.yml'
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo 'Cloning application repository...'
                git branch: 'main', url: 'https://github.com/Pratik-Pardeshi/new-ansible.git'
                sh 'cp -r * /home/jenkins/app/' // Copy repo files to app folder
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                echo 'Running Ansible playbook...'
                sh '''
                    ansible-playbook -i ${INVENTORY_FILE} ${PLAYBOOK_FILE}
                '''
            }
        }
    }
}
