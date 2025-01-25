pipeline {
    agent any
    environment {
        GIT_REPO = 'https://github.com/Pratik-Pardeshi/new-ansible.git'  // Your Git repository
        GIT_BRANCH = 'main'  // Adjust this if you're using a different branch
    }
    stages {
        stage('Checkout SCM') {
            steps {
                // Checkout the Git repository
                checkout scm: [
                    $class: 'GitSCM',
                    branches: [[name: "*/${GIT_BRANCH}"]],
                    userRemoteConfigs: [[url: "${GIT_REPO}"]]
                ]
            }
        }

        // Optional: Install dependencies if needed (skip if not applicable)
        stage('Install Dependencies') {
            steps {
                script {
                    echo 'Skipping dependency installation (requirements.txt not found or not needed).'
                }
            }
        }

        // Run the Ansible Playbook
        stage('Run Ansible Playbook') {
            steps {
                script {
                    // Running the Ansible playbook
                    echo 'Running the Ansible Playbook...'
                    sh 'ansible-playbook -i inventory.ini playbook.yml'
                }
            }
        }

        // Example Webserver test (replace with your actual test)
        stage('Test Webserver') {
            steps {
                script {
                    // Example test: Replace with your actual test command
                    echo 'Testing the Webserver setup...'
                    sh 'curl -f http://localhost'
                }
            }
        }
    }
    post {
        always {
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
