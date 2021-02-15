node("master") {
    stage("Checking out codebase") {
        checkout scm
        config = readProperties file: 'Configuration.properties'
    }
    packageHelmChart(
        helmChartDir: "${config.helm_chart_dir}"
        helmChartVersion: "${config.helm_chart_version}"
    )
    storeHelmChart(
        helmChartDir: "${config.helm_chart_dir}",
        applicationName: "${config.application_name}"
        helmChartVersion: "${config.helm_chart_version}"
    )
    testHelmChart(
//        helmChartDir: "${config.helm_chart_dir}",
//        applicationName: "${config.application_name}"
    )
    notificationStage(
        status: "good",
        environment: "dev",
        message: "New version app is successfully deployed"
    )
}

def packageHelmChart(Map stepParams) {
    try {
        stage("Packaging helm chart for application") {
            dir("${stepParams.helmChartDir}") {
                sh "helm package ./ --version ${stepParams.helmChartVersion}"
            }
        }
    } catch (Exception e) {
        echo "There is an error while packaging helm chart. Please check the logs!!!!"
        echo e.toString()
        throw e
    }
}

def storeHelmChart(Map stepParams) {
    try {
        stage("Storing the Helm chart") {
            dir("${stepParams.helmChartDir}") {
                sh "curl -uadmin:APAP3ArKZtCBVsPARwg4nZmiTng -T   ${stepParams.applicationName}-${stepParams.helmChartVersion}.tgz \"http://127.0.0.1:8081/artifactory/helm-local-artifactory/\""
            }
        }
    } catch (Exception e) {
        echo "There is an error while setting up application. Please check the logs!!!!"
        echo e.toString()
        throw e
    }
}

def testHelmChart(Map stepParams) {
    try {
        stage("Testing GoWebApp Application") {
            dir("${stepParams.helmChartDir}") {
                sh "helm test ${stepParams.applicationName} --namespace go-webapp"
            }
        }
    } catch (Exception e) {
        echo "There is an error while testing application. Please check the logs!!!!"
        echo e.toString()
        throw e
    }
}

def notificationStage(Map stepParams) {
    stage("Sending notification") {
        slackSend channel: 'build-status', color: "${stepParams.status}", message: "ENVIRONMENT:- ${stepParams.environment}\n BUILD_ID:- ${env.BUILD_ID}\n JOB_NAME:- ${env.JOB_NAME}\n Message:- ${stepParams.message}\n BUILD_URL:- ${env.BUILD_URL}"
    }
}
