pipeline {
  agent any

  stages {
    stage('Clone repository') {
      steps {
        git 'https://github.com/alexfbasa/learning_openshift.git'
      }
    }

    stage('Run command') {
      steps {
        sh 'ls -la'
        sh 'echo "Hello, Jenkins!"'
      }
    }
  }
}
