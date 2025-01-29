pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/pratik-pardeshi/ansible-simple.git'
            }
        }

        stage('Copy Files to Ansible Server') {
    steps {
        sh 'ssh -i /var/lib/jenkins/.ssh/id_rsa ansible@172.31.14.31 "sudo mkdir -p /home/ec2-user/ansible-project"'
        sh 'scp -i /var/lib/jenkins/.ssh/id_rsa -r Jenkinsfile ansible inventory.ini playbook.yml roles ansible@172.31.14.31:/home/ec2-user/ansible-project/'
    }
}


        stage('Run Ansible Playbook') {
            steps {
                sh 'ssh -i /var/lib/jenkins/.ssh/id_rsa ansible@172.31.14.31 "ansible-playbook /home/ec2-user/ansible-project/playbook.yml"'
            }
        }
    }
}
