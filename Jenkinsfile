pipeline {
    agent any 
    tools {
        terraform 'terraform'
    }
    stages{
        stage ('Github Checkout') {
            steps {
                git credentialsId: 'rajida', url: 'https://github.com/raji2306/Thoughtworks-mediawikiApplcation'
            }
        }
        stage ('Terraform init') {
            steps {
                sh 'terraform init'
            }
        }
        stage ('Terraform apply') {
            steps {
                sh 'terraform apply --auto-approve'
            }
        }
    }
} 
