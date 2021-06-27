pipeline {
  agent {
    kubernetes {
      defaultContainer 'docker'
      yaml '''
      apiVersion: v1
      kind: Pod
      metadata:
        namespace: jenkins
      spec:
        volumes:
        - name: docker-socket
          hostPath:
            path: /var/run/docker.sock
        containers:
        - image: docker:20-dind
          name: docker
          securityContext:
            privileged: true
          volumeMount:
          - mountPath: /var/run/docker.sock
            name: docker-socket
      '''
    }
  }
  stages {
    stage('Build') {
      steps {
        dir('chrome') {
          sh 'docker build -t kinerp/node:lts-chrome .'
        }
      }
    }
    stage('Push') {
      steps {
        withDockerRegistry(registry: [credentialsId: 'DOCKER_IMAGE_UPLOAD_TOKEN']) {
          sh 'docker push  kinerp/node:lts-chrome'
        }
      }
    }
  }
}
