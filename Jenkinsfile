pipeline {
    agent {
        label 'AGENT-1'
    }
    options {
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds()
        ansiColor('xterm')
    }
    parameters {
        string(name: 'appVersion', defaultValue: '1.0.0', description: 'what is the application version?')
        choice(name: 'action', choices:['Apply','Destroy'], description: 'pick something')
    }
    environment {
        def appVersion = '' //declaring global variable
        nexusUrl = 'nexus.devops76.sbs:8081'
    }
    stages {
        stage('Print the version'){
            steps{
                script{
                    // def packageJson = readJSON file: 'package.Json'
                    // appVersion = packageJSON.version 
                    echo "Application version: ${params.appVersion}"
                }
            }
        }
        stage('Init'){
            steps{
                sh """
                    cd terraform
                    terraform init
                """
            }
        }
        stage('plan'){
            when {
                expression {
                    params.action == 'Apply'
                }
            }
            steps{
                sh """
                    pwd
                    cd terraform
                    terraform plan -var="app_version=${params.appVersion}"
                """
            }
        }
        stage('Deploy'){
            when {
                expression {
                    params.action == 'Apply'
                }
            }
            input {
                message "Should we continue?"
                ok "Yes, we should"
            }
            steps{
                sh """
                    cd terraform
                    terraform apply -auto-approve -var="app_version=${params.appVersion}"
                """
            }
        }
        stage('Destroy'){
            when {
                expression {
                    params.action == 'Destroy'
                }
            }
            steps{
                sh """
                    cd terraform
                    terraform destroy -auto-approve -var="app_version=${params.appVersion}"
                """
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