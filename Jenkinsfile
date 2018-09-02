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

        sh 'find /usr/lib -maxdepth 1 -type d
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
  }
  post {
    always {
      sshagent (credentials: ['qemu']) {
        sh 'molecule --debug destroy'
      }
    }
  }
}
