pipeline {
    agent any

    environment {
        ANSIBLE_SERVER = 'ansible@172.31.14.31'
        GIT_REPO = 'https://github.com/Pratik-pardeshi/ansible-simple.git'
        DEPLOY_YML = 'deploy.yml'
    }

    stages {
        stage('Clone Git Repository') {
            steps {
                // Clone the repository and pull the necessary files
                git branch: 'main', url: "${GIT_REPO}"
            }
        }

        stage('Copy Files to Ansible Server') {
            steps {
                // Copy index.html and deploy.yml to the Ansible server
                sh 'scp -i /var/lib/jenkins/.ssh/id_rsa -r index.html ${ANSIBLE_SERVER}:/home/ec2-user/ansible-project/'
                sh 'scp -i /var/lib/jenkins/.ssh/id_rsa -r ${DEPLOY_YML} ${ANSIBLE_SERVER}:/home/ec2-user/ansible-project/'
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                // Run the deploy.yml playbook on the Ansible server
                sh 'ssh -i /var/lib/jenkins/.ssh/id_rsa ${ANSIBLE_SERVER} "ansible-playbook /home/ec2-user/ansible-project/deploy.yml"'
            }
        }
    }
}
