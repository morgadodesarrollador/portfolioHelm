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
    stage('Update Helm values') {
  steps {
    withCredentials([usernamePassword(
      credentialsId: 'github-creds',
      usernameVariable: 'GIT_USER',
      passwordVariable: 'GIT_TOKEN'
    )]) {

      sh """
      git clone https://$GIT_USER:$GIT_TOKEN@github.com/morgadodesarrollador/portfolioHelm.git helmrepo
      cd helmrepo/charts/portfolio

      sed -i "s/tag:.*/tag: ${GIT_COMMIT}/" values.yaml

      git config user.email "ci@jenkins"
      git config user.name "jenkins"

      git commit -am "Update image tag to ${GIT_COMMIT}"
      git push
      """
    }
  }
}

  }
}
