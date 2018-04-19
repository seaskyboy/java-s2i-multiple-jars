pipeline {
  agent any
  stages {
    stage('*** Create pod from template') {
      steps {
        script {
          openshift.withCluster() {
            openshift.withProject(DEV_PROJECT) {
              openshift.newApp("-f", TEMPLATE, "-p", "APPLICATION_NAME=${APPNAME}")
            }
          }
        }
      }
    }
  }
  environment {
    DEV_PROJECT = 'openjdk-app-pipe'
    TEMPLATE = 'openjdk18-web-basic-s2i-glowroot.yml'
    APPNAME = 'openjdk-app'
  }
}