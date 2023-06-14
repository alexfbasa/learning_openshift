pipeline {
  agent any

  tools {
    // Define the Git installation
    git 'git'
  }

  stages {
    stage('Clone repository') {
      steps {
        git 'https://github.com/alexfbasa/learning_openshift.git'
      }
    }

    // Other stages in your pipeline...
  }
}