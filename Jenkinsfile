pipeline {
    agent { label 'agentfarm' }
    stages {
        stage('Delete the workspace') {
      steps {
        cleanWs()
      }
        }
        stage('Installing Ansible') {
      steps {
        script {
            def ansible_exists = fileExists'/usr/bin/ansible'

            if (ansible_exists) {
                echo 'Ansible is already installed'
            } else {
                sh 'sudo apt-get update -y && sudo apt-get upgrade -y'
                sh 'sudo apt install -y wget tree unzip ansible python3-pip python3-apt'
            }
        }
      }
        }
        stage('Download Ansible Code') {
      steps {
        git credentialsId: 'git-repo-creds-second', url: 'git@github.com:Cfloz/ansible-webservers.git'
      }
        }
        stage('Run ansible-lint against playbook') {
          steps {
            sh 'docker run --rm -v $WORKSPACE/playbook:/data cytopia/ansible-lint:4 apache-install.yml'
            sh 'docker run --rm -v $WORKSPACE/playbook:/data cytopia/ansible-lint:4 website-update.yml'
            sh 'docker run --rm -v $WORKSPACE/playbook:/data cytopia/ansible-lint:4 website-test.yml'
          }
        }
    }
}
