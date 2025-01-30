pipeline {
    agent any

    stages {
        stage('Create Files on Jenkins') {
            steps {
                script {
                    sh '''
                    rm -rf /tmp/deployment && mkdir -p /tmp/deployment
                    echo "<h1>Deployed via Jenkins</h1>" > /tmp/deployment/index.html
                    cat <<EOL > /tmp/deployment/playbook.yml
                    - name: Deploy Web Application
                      hosts: webserver
                      become: yes
                      tasks:
                        - name: Copy index.html to Web Server
                          copy:
                            src: /home/ansible/deployment/index.html
                            dest: /var/www/html/index.html
                            mode: '0644'
                    EOL
                    ls -l /tmp/deployment/
                    '''
                }
            }
        }

        stage('Ensure Deployment Directory on Ansible Server') {
            steps {
                script {
                    sh '''
                    ssh -o StrictHostKeyChecking=no ansible@172.31.34.55 "
                    sudo mkdir -p /home/ansible/deployment &&
                    sudo chown -R ansible:ansible /home/ansible/deployment &&
                    sudo chmod -R 777 /home/ansible/deployment &&
                    ls -ld /home/ansible/deployment/"
                    '''
                }
            }
        }

        stage('Copy Files to Ansible Server') {
            steps {
                script {
                    sh '''
                    scp -o StrictHostKeyChecking=no /tmp/deployment/index.html ansible@172.31.34.55:/home/ansible/deployment/
                    scp -o StrictHostKeyChecking=no /tmp/deployment/playbook.yml ansible@172.31.34.55:/home/ansible/deployment/
                    '''
                }
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                script {
                    sh "ssh -o StrictHostKeyChecking=no ansible@172.31.34.55 'ansible-playbook /home/ansible/deployment/playbook.yml'"
                }
            }
        }
    }
}
