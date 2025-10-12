pipeline {
    agent {
        lable 'AGENT-1'
    }
    options {
        timeout(time: 30, unit: 'MINUTES')
    }
    parameters {
        string(name: 'appVersion', defaultValue: '1.0.0', description: 'what is the application version')
    }
    environment {
        def appVersion: '' //declaring global variable
        nexusUrl = 'nexus.devops76.sbs:8081'
    }
    stages {
        stage('Print the version'){
            steps {
                script {
                    // def packageJson = readJSON file: 'package.Json'
                    // appVersion = packageJSON.version 
                    echo "application version: ${params.appVersion}"
                }
            }
        }
    }
    post {
        always {
            echo 'I will always say hello again!'
            deleteDir()
        }
        success{
            echo 'i will run when pipeline is success'
        }
        failure{
            echo 'i will run when pipeline is failure'
        }
    }
}