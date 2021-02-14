pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'This step compiles and builds docker file '
      }
    }

    stage('Store') {
      steps {
        sh '''helm package .

curl -uadmin:APAP3ArKZtCBVsPARwg4nZmiTng -T  persons-ms/persons-ms-0.2.0.tgz "http://127.0.0.1:8081/artifactory/helm-local-artifactory/"'''
      }
    }

  }
}