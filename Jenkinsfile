pipeline {
    agent any

    environment {
        ANSIBLE_SERVER = '172.31.14.31'
        GIT_REPO = 'https://github.com/pratik-pardeshi/ansible-simple.git'
        DEPLOY_YML = 'deploy.yml'
        INDEX_HTML = 'index.html'
        TARGET_DIR = '/home/ec2-user/ansible-project'
    }

    stages {
        stage('Clone Git Repository') {
            steps {
                // Clone the repository and fetch the files
                git branch: 'main', url: "${GIT_REPO}"
            }
        }

        stage('Copy Files to Ansible Server') {
            steps {
                // Create a new directory on the Ansible server
                sh 'ssh -i /var/lib/jenkins/.ssh/id_rsa ${ANSIBLE_SERVER} "mkdir -p ${TARGET_DIR}"'

                // Copy index.html and deploy.yml to the new directory on the Ansible server
                sh 'scp -i /var/lib/jenkins/.ssh/id_rsa -r index.html ${ANSIBLE_SERVER}:${TARGET_DIR}/'
                sh 'scp -i /var/lib/jenkins/.ssh/id_rsa -r ${DEPLOY_YML} ${ANSIBLE_SERVER}:${TARGET_DIR}/'
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                // Run the deploy.yml playbook from the new directory on the Ansible server
                sh 'ssh -i /var/lib/jenkins/.ssh/id_rsa ${ANSIBLE_SERVER} "ansible-playbook ${TARGET_DIR}/deploy.yml"'
            }
        }
    }
}
