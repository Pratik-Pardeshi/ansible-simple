pipeline {
    agent any
    environment {
        GIT_REPO = 'https://github.com/Pratik-Pardeshi/new-ansible.git'
        GIT_BRANCH = 'main'  // Update this if your branch name is different
    }
    stages {
        stage('Checkout SCM') {
            steps {
                // Checkout the code from GitHub repository
                checkout scm: [
                    $class: 'GitSCM',
                    branches: [[name: "*/${GIT_BRANCH}"]],
                    userRemoteConfigs: [[url: "${GIT_REPO}"]]
                ]
            }
        }
        
        stage('Install Dependencies') {
            steps {
                script {
                    // Example to install dependencies
                    // Replace with your actual commands
                    sh 'pip install -r requirements.txt' 
                }
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                script {
                    // Running your Ansible playbook
                    // Replace this with your actual playbook run command
                    sh 'ansible-playbook -i inventory.ini playbook.yml'
                }
            }
        }

        stage('Test Webserver') {
            steps {
                script {
                    // Example test: Replace with your actual test
                    sh 'curl -f http://localhost'
                }
            }
        }
    }
    post {
        always {
            // Clean up or other final steps if needed
            echo 'Pipeline execution completed.'
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
