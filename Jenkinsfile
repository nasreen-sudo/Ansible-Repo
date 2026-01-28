pipeline {
    agent any
 
    tools {
        ansible 'ansible'
    }
 
    stages {
 
        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    credentialsId: 'git-id',
                    url:'https://github.com/nasreen-sudo/Ansible-Repo.git'
            }
        }
 
        stage('Deploy with Ansible') {
            steps {
                withCredentials([sshUserPrivateKey(
                    credentialsId: 'ssh-id',
                    keyFileVariable: 'SSH_KEY'
                )]) {
 
                    echo 'Deploying website using Ansible...'
 
                    sh 'cp ${SSH_KEY} /tmp/ssh_key'
                    sh 'chmod 600 /tmp/ssh_key'
 
                    ansiblePlaybook(
                        playbook: 'deploy.yml',
                        inventory: 'inventory/hosts.ini',
                        colorized: true
                    )
 
                    sh 'rm -f /tmp/ssh_key'
                }
            }
        }
    }
}
