def templatePath = 'openjdk18-web-basic-s2i-glowroot.yml'
def appName = 'openjdk-app'

pipeline {
  agent any
  stages { 
    stage('Create Image') {
      steps {
        script {
            openshift.withCluster() {
                openshift.withProject() {
                  openshift.newApp("-f", templatePath, "-p", "APPLICATION_NAME=${appName}")
                }
            }
        }
        script {
          openshift.withCluster() {
            openshift.withProject() {
              def builds = openshift.selector("bc", appName).related('builds')
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
    stage('Deploy Stage') {
      steps {
        script {
            openshift.withCluster() {
                openshift.withProject() {
                  def rm = openshift.selector("dc", appName).rollout()
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
}
