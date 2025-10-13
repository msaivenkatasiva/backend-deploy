pipeline {
  agent { label 'AGENT-1' }
  options {
    timeout(time: 30, unit: 'MINUTES')
    disableConcurrentBuilds()
    ansiColor('xterm')
  }
  parameters {
    string(name: 'appVersion', defaultValue: '', description: 'App version from upstream')
  }
  environment {
    APP_VERSION = "${params.appVersion}"    // expose to all steps and sh
    NEXUS_URL   = 'nexus.devops76.sbs:8081'
  }
  stages {
    stage('Print the version') {
      steps {
        echo "Received APP_VERSION: ${env.APP_VERSION}"
        sh 'echo APP_VERSION=$APP_VERSION'
      }
    }
    stage('Init Terraform') {
      steps {
        dir('terraform') {
          sh 'terraform init -input=false'
        }
      }
    }
    stage('Plan') {
      steps {
        dir('terraform') {
          // Ensure variable "app_version" exists in your TF code:
          // variable "app_version" { type = string }
          sh "terraform plan -input=false -var app_version=${env.APP_VERSION}"
        }
      }
    }
    // Uncomment to deploy
    // stage('Apply') {
    //   steps {
    //     dir('terraform') {
    //       sh "terraform apply -auto-approve -input=false -var app_version=${env.APP_VERSION}"
    //     }
    //   }
    // }
  }
  post {
    always  { echo 'Cleanup'; deleteDir() }
    success { echo 'Downstream succeeded' }
    failure { echo 'Downstream failed' }
  }
}