pipeline {
  agent any
  environment {
    DH_CREDS=credentials('jfrog-plugin')
  }
  stages {
    stage('Remove all images from agent') {
      steps {
        sh 'podman rmi --all --force'
      }
    }
    stage('build image') {
      steps {
        sh 'podman build -t haplolabs.jfrog.io/default-docker-virtual/haplolabs/haplolabs-hello-world:2024-02-19 .'
      }
    }
    stage('Login to Docker Hub') {
      steps {
        sh 'echo $DH_CREDS_PSW | podman login -u $DH_CREDS_USR --password-stdin https://haplolabs.jfrog.io'
      }
    }
    stage('Tag the image') {
      steps {
        sh 'podman tag haplolabs.jfrog.io/default-docker-virtual/haplolabs/haplolabs-hello-world:2024-02-19 haplolabs.jfrog.io/default-docker-virtual/haplolabs/haplolabs-hello-world:latest'
      }
    }
    stage('Push the image') {
      steps {
        sh '''
          podman push haplolabs.jfrog.io/default-docker-virtual/haplolabs/haplolabs-hello-world:2024-02-19
          podman push haplolabs.jfrog.io/default-docker-virtual/haplolabs/haplolabs-hello-world:latest
        '''
      }
    }
  }
  post {
    always {
      sh 'podman logout https://haplolabs.jfrog.io'
    }
  }
}
