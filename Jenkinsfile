pipeline {
    agent any

    stages {
        stage('Create Files on Jenkins') {
            steps {
                script {
                    sh '''
                    # Ensure the deployment directory exists
                    mkdir -p /tmp/deployment

                    # Create index.html file
                    echo "<h1>Deployed via Jenkins</h1>" > /tmp/deployment/index.html

                    # Create playbook.yml
                    cat <<EOL > /tmp/deployment/playbook.yml
                    - name: Deploy Web Application
                      hosts: localhost
                      become: yes
                      tasks:
                        - name: Copy index.html to Web Server
                          copy:
                            src: /home/ansible/deployment/index.html
                            dest: /var/www/html/index.html
                            mode: '0644'
                    EOL

                    # Debugging step to ensure files exist
                    ls -l /tmp/deployment/
                    '''
                }
            }
        }

        stage('Ensure Deployment Directory on Ansible Server') {
            steps {
                script {
                    sh '''
                    ssh -o StrictHostKeyChecking=no ansible@172.31.34.144 "
                    sudo mkdir -p /home/ansible/deployment &&
                    sudo chown -R ansible:ansible /home/ansible/deployment &&
                    sudo chmod -R 777 /home/ansible/deployment &&
                    ls -ld /home/ansible/deployment/"
                    "
                    '''
                }
            }
        }

        stage('Copy Files to Ansible Server') {
            steps {
                script {
                    sh '''
                    # Copy files from Jenkins to Ansible Server
                    scp -o StrictHostKeyChecking=no /tmp/deployment/index.html ansible@172.31.34.144:/home/ansible/deployment/
                    scp -o StrictHostKeyChecking=no /tmp/deployment/playbook.yml ansible@172.31.34.144:/home/ansible/deployment/

                    # Verify files exist on the Ansible server
                    ssh -o StrictHostKeyChecking=no ansible@172.31.34.144 "ls -l /home/ansible/deployment/"
                    '''
                }
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                script {
                    sh '''
                    ssh -o StrictHostKeyChecking=no ansible@172.31.34.144 "
                    ansible-playbook /home/ansible/deployment/playbook.yml
                    "
                    '''
                }
            }
        }
    }
}
