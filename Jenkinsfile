pipeline {
    agent any

    environment {
        ANSIBLE_SERVER = 'ansible@172.31.14.31'
        GIT_REPO = 'https://github.com/pratik-pardeshi/ansible-simple'
        DEPLOY_YML = 'deploy.yml'
        TARGET_DIR = '/home/jenkins/ansible-project/' // Updated directory
    }

    stages {
        stage('Clone Git Repository') {
            steps {
                // Clone the repository containing deploy.yml
                git branch: 'main', url: "${GIT_REPO}"
            }
        }

        stage('List Workspace') {
            steps {
                // List the current workspace to confirm the file's location
                sh 'pwd'
                sh 'ls -alh'
            }
        }

        stage('Copy deploy.yml to Ansible Server') {
            steps {
                // Copy deploy.yml to /home/jenkins/ansible-project/ on the Ansible server
                sh 'scp -i /var/lib/jenkins/.ssh/id_rsa ${DEPLOY_YML} ${ANSIBLE_SERVER}:${TARGET_DIR}'
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                // SSH into the Ansible server and run the playbook
                sh 'ssh -i /var/lib/jenkins/.ssh/id_rsa ${ANSIBLE_SERVER} "ansible-playbook ${TARGET_DIR}${DEPLOY_YML}"'
            }
        }
    }
}
