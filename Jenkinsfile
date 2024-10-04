pipeline {
    agent any 
    tools {
        "org.jenkinsci.plugins.terraform.TerraformInstallation" "terraform"
    }
    environment {
        TF_HOME = tool('terraform')
        TF_IN_AUTOMATION = "true"
        PATH = "$TF_HOME:$PATH"
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the source code from version control
                checkout scm
            }
        }

        stage('Terraform Init') {
            steps {
                dir("${env.TF_DIR}") {
                    // Initialize Terraform
                    sh 'terraform init'
                }
            }
        }

        stage('Terraform Validate') {
            steps {
                dir("${env.TF_DIR}") {
                    // Validate the Terraform configuration
                    sh 'terraform validate'
                }
            }
        }

        stage('Terraform Plan') {
            steps {
                dir("${env.TF_DIR}") {
                    // Create a Terraform execution plan
                    sh 'terraform plan'
                }
            }
        }

        stage('Terraform Apply') {
            steps {
                dir("${env.TF_DIR}") {
                    // Apply the Terraform changes
                    // The `-auto-approve` flag skips interactive approval
                    sh 'terraform apply -auto-approve'
                }
            }
        }

        stage('Clean Up') {
            steps {
                dir("${env.TF_DIR}") {
                    // Optionally, you can add cleanup steps here
                    // e.g., removing temporary files or state files
                }
            }
        }
    }

    post {
        success {
            echo 'Terraform applied successfully!'
        }
        failure {
            echo 'Terraform application failed.'
        }
        always {
            // Clean up any artifacts if necessary
            cleanWs()
        }
    }
}
