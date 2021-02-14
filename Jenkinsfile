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
        def props = readProperties(file: 'Configuration.properties')
        echo "running for the app  ${props.application_name}"
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
