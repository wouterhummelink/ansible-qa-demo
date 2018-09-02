pipeline {
  agent {
    kubernetes {
      defaultContainer 'ansible'
      yamlFile 'KubernetesPod.yml'
    }
  }
  stages {
    stage('molecule dependency') {
      steps {
        sh 'molecule dependency'
      }
    }
    stage('molecule syntax') {
      steps {
        sh 'molecule syntax'
      }
    }
    stage('molecule create') {
      steps {
        sh '#molecule create'
      }
    }
  }
}
