pipeline {
    agent any
    
    tools {
        terraform'terraform'
        ansible'ansible'
    }

    environment {
        AWS_DEFAULT_REGION = "us-east-1"
        AWS_CONFIGURE = credentials('AWS_Creds')
        STACK_NAME = "myEc2Stack"
    }

    stages {
        stage('git checkout') {
            steps{
                sh "git clone "
            }
        }
        stage('aws setup Check') {
            steps {
                sh 'aws sts get-caller-identity'
            }
        }
        stage('CloudFormation stack Provision') {
            steps {

                sh "aws cloudformation create-stack --stack-name ${env.STACK_NAME} --template-body file://app.yaml"
            }
        }
        stage('Wait for CloudFormation Stack Creation') {
            steps {

                sh "aws cloudformation wait stack-create-complete --stack-name ${env.STACK_NAME}"
            }
        }
        stage('Configure web app with ansible') {
            steps {

                sh 'ansible-playbook -i aws_ec2.yaml play.yaml --private-key=${Private_key}'
            }
        }
    }
}
