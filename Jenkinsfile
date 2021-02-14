properties = null

def loadProperties() {
    node {
        checkout scm
        config = readProperties file: 'Configuration'
        echo "Immediate one ${config.application_name}"
    }
}

pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'This step compiles and builds docker file '
      }
    }

    stage('Package') {
      steps {
        
        echo 'running for the app  ${config.application_name}'
        sh '''helm package .

curl -uadmin:APAP3ArKZtCBVsPARwg4nZmiTng -T  persons-ms/persons-ms-0.2.0.tgz "http://127.0.0.1:8081/artifactory/helm-local-artifactory/"'''
      }
    }

    stage('Store') {
      steps {
        sh 'TBD'
      }
    }

  }
}