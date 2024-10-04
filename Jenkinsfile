pipeline {
  agent any 
    tools {
        "org.jenkinsci.plugins.terraform.TerraformInstallation" "terraform"
    }

  options {
    ansiColor('xterm')
  }

  environment {
    BRANCH = 'main'
    REPO = 'https://github.com/AnuragKishor-Git/azureUdemy.git'

    ARM_ACCESS_KEY = credentials('ARM_ACCESS_KEY')
    ARM_CLIENT_ID = credentials('ARM_CLIENT_ID')
    ARM_CLIENT_SECRET = credentials('ARM_CLIENT_SECRET')
    ARM_SUBSCRIPTION_ID = credentials('ARM_SUBSCRIPTION_ID')
    ARM_TENANT_ID = credentials('ARM_TENANT_ID')
  }

  stages {

    stage('Checkout Source') {

      steps {
        git branch: "$BRANCH",
            url: "$REPO"
      }
    }

    stage('Terraform - init') {
      steps {
          sh 'terraform init -upgrade -input=false'
      }
    }

    stage('Terraform - validate') {
      steps {
          sh 'terraform validate'
      }
    }

    stage('Terraform - tfsec') {
      steps {
          sh 'tfsec .'
      }
    }

    stage('Terraform - plan') {
      steps {
          sh 'terraform plan -out=tfplan -input=false'
      }
    }

    stage('Terraform - apply') {
      steps {
          sh 'terraform apply -input=false tfplan'
      }
    }

  }

  post {
    always {
      cleanWs()
    }
  }
} // pipeline
