pipeline {
  options {
    disableConcurrentBuilds()
  }
  agent {
    label "jenkins-maven"
  }
  environment {
    DEPLOY_NAMESPACE = "teama-staging"
    TILLER_NAMESPACE = "kube-system"
    HTTP_PROXY = "http://172.22.100.64:5865"
    NO_PROXY = "jenkins-x*,.local,jenkins-x-chartmuseum,10.0.0.0/8"
  }
  stages {
    stage('Validate Environment') {
      steps {
        container('maven') {
          dir('env') {
            sh 'helm init --client-only --service-account tiller'
            sh 'jx step helm build'
          }
        }
      }
    }
    stage('Update Environment') {
      when {
        branch 'master'
      }
      steps {
        container('maven') {
          dir('env') {
            sh 'jx step helm apply'
          }
        }
      }
    }
  }
}
