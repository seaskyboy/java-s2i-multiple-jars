pipeline {
  agent any
  stages {
    stage('*** Clean up project') {
      steps {
        script {
          openshift.withCluster() {
            openshift.withProject(DEV_PROJECT) {
              openshift.delete("all", "-l", "application=${APPNAME}")
            }
          }
        }
      }
    }
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
    stage('*** Build image') {
      steps {
        script {
          openshift.withCluster() {
            openshift.withProject(DEV_PROJECT) {
              def builds = openshift.selector("bc", APPNAME).related('builds')
              timeout(6) {
                builds.untilEach(1) {
                  return (it.object().status.phase == "Complete")
                }
              }
            }
          }
        }
      }
    }
    stage('*** Deploy pod') {
      steps {
        script {
          openshift.withCluster() {
            openshift.withProject(DEV_PROJECT) {
              def rm = openshift.selector("dc", APPNAME).rollout()
              timeout(5) {
                openshift.selector("dc", appName).related('pods').untilEach(1) {
                  return (it.object().status.phase == "Running")
                }
              }
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