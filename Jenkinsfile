pipeline {
    agent any
    
    // tools {
    //     terraform'terraform'
    //     ansible'Ansible'
    // }

    environment {
        AWS_DEFAULT_REGION = "us-east-1"
        AWS_CONFIGURE = credentials('AWS_Creds')
        STACK_NAME = "myEc2Stack"
        SSH_KEY_CREDENTIAL = credentials('Private_key')
    }

    stages {
        stage('git checkout') {
            steps{
                sh "rm -rf Ansible-CTF-Jenkins"
                sh "git clone https://github.com/vinaynagamuntala/Ansible-CTF-Jenkins.git"

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
        stage('Setting up webapp with ansible') {
            steps {
                sh "ansible-playbook play.yaml -i aws_ec2.yaml --private-key ${SSH_KEY_CREDENTIAL}"
            }
        }
    }
}
