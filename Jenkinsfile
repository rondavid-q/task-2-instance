pipeline {
    agent any
    
    parameters {
        choice(name: 'ACTION', choices: ['apply', 'destroy'], description: 'Terraform Action')
        string(name: 'INSTANCE_NAME', defaultValue: 'task-test', description: 'Enter Instance Name')
        choice(name: 'REGION', choices: ['ap-south-1', 'us-east-1'], description: 'Select Region')
        string(name: 'AMI_ID', defaultValue: 'ami-0287a05f0ef0e9d9a', description: 'Enter AMI ID')
        string(name: 'INSTANCE_TYPE', defaultValue: 't2.micro', description: 'Enter Instance Type')
    }
    
    stages{
        stage('Git Checkout') {
            steps {
                git ( url: "https://github.com/rondavid-q/task-2-instance.git", credentialsId: "github", branch: "main" )
            }
        }
        stage('Terraform Initialization') {
            steps {
                sh "terraform init -reconfigure" 
            }
        }
        stage('Terraform Workspace') {
            steps {
                script {
                    // Check if the workspace already exists
                    def workspaceExists = sh(script: "terraform workspace list | grep -q ${params.INSTANCE_NAME}", returnStatus: true)

                    if (workspaceExists == 0) {
                        // If the workspace exists, select it
                        sh "terraform workspace select ${params.INSTANCE_NAME}"
                    } else {
                        // If the workspace doesn't exist, create it
                        sh "terraform workspace new ${params.INSTANCE_NAME}"
                    }
                }
            }
        }
        stage('Plan') {
            steps {
                script {
                    withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: "aws-key", secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
                    sh "terraform plan -var='instance_name=${params.INSTANCE_NAME}' -var='region=${params.REGION}' -var='ami_id=${params.AMI_ID}' -var='instance_type=${params.INSTANCE_TYPE}' -var='key_name=${params.INSTANCE_NAME}.pem' "
                    }
                }
            }
        }
        stage('Action') {
            steps {
                script {
                    withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: "aws-key", secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
                    sh ("terraform ${params.ACTION} -var='instance_name=${params.INSTANCE_NAME}' -var='region=${params.REGION}' -var='ami_id=${params.AMI_ID}' -var='instance_type=${params.INSTANCE_TYPE}' -var='key_name=${params.INSTANCE_NAME}.pem' --auto-approve")
                    }
                }
            }
        }
    }
}