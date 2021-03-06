node("master") {
    stage("Checking out codebase") {
        checkout scm
        config = readProperties file: 'Configuration.properties'
    }
    packageHelmChart(
        helmChartDir: "${config.helm_chart_dir}",
        helmChartVersion: "${config.helm_chart_version}"
    )
    storeHelmChart(
        helmChartDir: "${config.helm_chart_dir}",
        applicationName: "${config.application_name}",
        helmChartVersion: "${config.helm_chart_version}"
    )
    applyHelmChartDev(
        helmChartDir: "${config.helm_chart_dir}",
        applicationName: "${config.application_name}"
    )  
    applyHelmChartQa(
        helmChartDir: "${config.helm_chart_dir}",
        applicationName: "${config.application_name}"
    )
    applyHelmChartProd(
        helmChartDir: "${config.helm_chart_dir}",
        applicationName: "${config.application_name}"
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
                sh "/usr/local/bin/helm package ./ --version ${stepParams.helmChartVersion}"
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

def applyHelmChartDev(Map stepParams) {
    try {
        stage("Deploying the Helm Chart to DEV") {
            dir("${stepParams.helmChartDir}") {
                input "Deploy to DEV?"
                sh "/usr/local/bin/helm upgrade ${stepParams.applicationName} ./ -f values-dev.yaml --namespace helm-dev --install"
            }
        }
    } catch (Exception e) {
        echo "There is an error while setting up application. Please check the logs!!!!"
        echo e.toString()
        throw e
    }
}

def applyHelmChartQa(Map stepParams) {
    try {
        stage("Deploying the Helm Chart to QA") {
            dir("${stepParams.helmChartDir}") {
                input "Deploy to QA?"
                sh "/usr/local/bin/helm upgrade ${stepParams.applicationName} ./ -f values-qa.yaml --namespace helm-qa --install"
            }
        }
    } catch (Exception e) {
        echo "There is an error while setting up application. Please check the logs!!!!"
        echo e.toString()
        throw e
    }
}

def applyHelmChartProd(Map stepParams) {
    try {
        stage("Deploying the Helm Chart to PROD") {
            dir("${stepParams.helmChartDir}") {
                input "Deploy to PROD?"
                sh "/usr/local/bin/helm upgrade ${stepParams.applicationName} ./ -f values-prod.yaml --namespace helm-prod --install"
            }
        }
    } catch (Exception e) {
        echo "There is an error while setting up application. Please check the logs!!!!"
        echo e.toString()
        throw e
    }
}

def notificationStage(Map stepParams) {
    stage("Sending notification") {
       // slackSend channel: 'build-status', color: "${stepParams.status}", message: "ENVIRONMENT:- ${stepParams.environment}\n BUILD_ID:- ${env.BUILD_ID}\n JOB_NAME:- ${env.JOB_NAME}\n Message:- ${stepParams.message}\n BUILD_URL:- ${env.BUILD_URL}"
    }
}
