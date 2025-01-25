pipeline {
    agent any
    environment {
        ANSIBLE_HOST_KEY_CHECKING = "False"  // Disable SSH key checking for simplicity
    }
    stages {
        stage('Clone Repository') {
            steps {
                // Clone the repository containing your Ansible playbook
                git 'https://your-repository-url.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    // Install necessary dependencies for Ansible
                    sh 'sudo apt-get update && sudo apt-get install -y python3-pip'
                    sh 'pip3 install ansible'
                }
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                script {
                    // Run the Ansible playbook to set up the webserver
                    sh 'ansible-playbook -i inventory ansible/setup-webserver.yml'
                }
            }
        }

        stage('Test Webserver') {
            steps {
                script {
                    // Check if Apache is serving the index page
                    def response = sh(script: 'curl -s http://192.168.1.10', returnStdout: true)
                    if (response.contains("Welcome to the Apache Webserver!")) {
                        echo "Webserver is deployed successfully!"
                    } else {
                        error "Webserver setup failed."
                    }
                }
            }
        }
    }
}
