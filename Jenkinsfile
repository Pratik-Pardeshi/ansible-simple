pipeline {
    agent any

    stages {
        stage('Clone Ansible Playbook') {
            steps {
                sh 'ssh -i /var/lib/jenkins/.ssh/id_rsa ansible@172.31.14.31 "rm -rf /home/ec2-user/ansible-project && git clone https://github.com/pratik-pardeshi/ansible-simple.git /home/ec2-user/ansible-project"'
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                sh 'ssh -i /var/lib/jenkins/.ssh/id_rsa ansible@172.31.14.31 "ansible-playbook /home/ec2-user/ansible-project/playbook.yml"'
            }
        }
    }
}
