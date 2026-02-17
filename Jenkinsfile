pipeline {
  agent {
    kubernetes {
      yaml """
  apiVersion: v1
  kind: Pod
  spec:
    containers:
    - name: kaniko
      image: gcr.io/kaniko-project/executor:debug
      command:
        - /busybox/cat
      tty: true
      volumeMounts:
        - name: docker-config
          mountPath: /kaniko/.docker

    volumes:
      - name: docker-config
        secret:
          secretName: dockerhub-secret
  """
    }
  }

  environment {
    IMAGE = "morgadoberruezo/miportfolio:latest"
  }

  stages {

    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build & Push Image') {
      steps {
        container('kaniko') {
          sh """
          /kaniko/executor \
            --context `pwd` \
            --dockerfile deploy/build_img/Dockerfile \
            --destination morgadoberruezo/portfolio:${GIT_COMMIT} \
            --destination morgadoberruezo/portfolio:latest \
            --snapshot-mode=redo \
            --single-snapshot
          """
        }
      }
    }
  }
}
