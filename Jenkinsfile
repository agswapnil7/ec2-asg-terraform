pipeline {
  agent any

  environment {
    TF_IN_AUTOMATION = "true"
  }

  stages {
    stage('Terraform Init') {
      steps {
        sh 'terraform init'
      }
    }
    stage('Terraform Plan') {
      steps {
        sh 'terraform plan'
      }
    }
    stage('Terraform Apply') {
      steps {
        sh 'terraform apply -auto-approve'
      }
    }
    stage('Wait') {
      steps {
        echo 'Sleeping for 2 minutes to simulate testing...'
        sh 'sleep 120'
      }
    }
    stage('Terraform Destroy') {
      steps {
        sh 'terraform destroy -auto-approve'
      }
    }
  }
}

