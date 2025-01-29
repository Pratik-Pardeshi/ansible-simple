pipeline {
    agent any

    environment {
        ANSIBLE_SERVER = 'ansible@172.31.14.31'
        GIT_REPO = 'https://github.com/pratik-pardeshi/ansible-simple'
        DEPLOY_YML = 'deploy.yml'
        TARGET_DIR = '/home/ec2-user/'
    }

    stages {
        stage('Clone Git Repository') {
            steps {
                // Clone the repository that contains deploy.yml
                git branch: 'main', url: "${GIT_REPO}"
            }
        }

        stage('Copy deploy.yml to Ansible Server') {
            steps {
                // Copy deploy.yml to /home/ec2-user/ directory on the Ansible server
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
