pipeline {
  agent any

  stages {
    stage('Checkout2') {
      steps {
        echo "Repositorio clonado correctamente"
        sh 'ls -la'
      }
    }

    stage('Info3') {
      steps {
        sh 'git rev-parse --short HEAD'
      }
    }
  }
}
