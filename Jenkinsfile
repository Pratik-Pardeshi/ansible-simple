pipeline {
    agent any

    stages {
        stage('Trigger Ansible') {
            steps {
                // SSH into the Ansible server and run the playbook
                sh 'ssh -i ~/.ssh/id_rsa ec2-user@172.31.14.31 "ansible-playbook /home/ec2-user/ansible-project/playbook.yml"'
            }
        }
    }
}
