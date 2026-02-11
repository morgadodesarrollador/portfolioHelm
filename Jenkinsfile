pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        echo "Repositorio clonado correctamente"
        sh 'ls -la'
      }
    }

    stage('Info2') {
      steps {
        sh 'git rev-parse --short HEAD'
      }
    }
  }
}
