pipeline {
  agent {
    kubernetes {
      label 'molecule-ansible'
      defaultContainer 'ansible'
      yamlFile 'KubernetesPod.yml'
    }
  }
  stages {
    stage('Prepare') {
      steps {
        sh 'ln -s ${WORKSPACE} molecule/default/testrole'
        sh 'if [ ! -d ~/.ssh ]; then mkdir ~/.ssh && chmod 700 ~/.ssh; fi'
        sh 'ssh-keyscan 192.168.122.1 > ~/.ssh/known_hosts'

        sh 'find /usr/lib -maxdepth 1 -type d'
      }
    }
    stage('molecule dependency') {
      steps {
        sh 'molecule dependency'
      }
    }
    stage('molecule lint') {
      steps {
        sh 'molecule lint'
      }
    }
    stage('molecule syntax') {
      steps {
        sh 'env' 
        sh 'ls -lR'
        sh 'molecule syntax'
      }
    }
    stage('molecule create') {
      steps {
        sshagent (credentials: ['qemu']) {
          sh 'molecule --debug create'
        }
      }
    }
    stage('molecule converge') {
      steps {
        sh 'molecule converge'
      }
    }
    stage('molecule idempotence') {
      steps {
        sh 'molecule idempotence'
      }
    }
    stage('molecule verify') {
      steps {
        sh 'molecule verify'
      }
    }
  }
  post {
    failure {
      fileExists('/tmp/molecule/ansible-qa-demo/default/vagrant-instance.err') {
        sh 'cat /tmp/molecule/ansible-qa-demo/default/vagrant-instance.err'
      }
    }
    always {
      sshagent (credentials: ['qemu']) {
        sh 'molecule destroy'
      }
    }
  }
}
