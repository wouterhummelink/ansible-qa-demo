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
          sh 'molecule create'
        }
      }
    }
  }
  post {
    always {
      sshagent (credentials: ['qemu']) {
        sh 'molecule destroy'
      }
    }
  }
}
