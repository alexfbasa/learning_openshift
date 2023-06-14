pipeline {
  agent any

  stages {
    stage('Clone repository') {
      steps {
        git 'https://github.com/alexfbasa/learning_openshift.git'
      }
    }

    stage('Build Image') {
      steps {
        sh 'docker build -t nginx_test .'
      }
    }
  }
}
