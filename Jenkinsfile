pipeline {
    agent any

    environment {
        ANSIBLE_SERVER = 'ansible@172.31.14.31'
        PLAYBOOK_PATH = '/home/ec2-user/deploy.yml'
    }

    stages {
        stage('Run Ansible Playbook') {
            steps {
                // SSH into the Ansible server and run the playbook
                sh 'ssh -i /var/lib/jenkins/.ssh/id_rsa ${ANSIBLE_SERVER} "ansible-playbook ${PLAYBOOK_PATH}"'
            }
        }
    }
}
