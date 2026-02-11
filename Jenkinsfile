pipeline {
  agent any

  stages {
    stage('Checkout1') {
      steps {
        echo "Repositorio clonado correctamente"
        sh 'ls -la'
      }
    }

    stage('Info') {
      steps {
        sh 'git rev-parse --short HEAD'
      }
    }
  }
}
