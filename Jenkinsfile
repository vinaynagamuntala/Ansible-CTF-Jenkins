pipeline {
    agent any
    
    // tools {
    //     ansible'Ansible'
    // }

    environment {
        AWS_DEFAULT_REGION = "us-east-1"
        AWS_CONFIGURE = credentials('AWS_Creds')
        SSH_KEY_CREDENTIAL = credentials('Private_key')
        STACK_NAME = "myEc2Stack"
        TEMPLATE_PATH = "app.yaml"
        PARAMETER_KEY = "Environment"
        PARAMETER_VALUE = "dev"
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

                //sh "aws cloudformation create-stack --stack-name ${env.STACK_NAME} --template-body file://app.yaml"
                sh "aws cloudformation deploy --template-file ${TEMPLATE_PATH} --stack-name ${STACK_NAME} --parameter-overrides ${PARAMETER_KEY}=${PARAMETER_VALUE}
"
            }
        }
        // stage('Wait for CloudFormation Stack Creation') {
        //     steps {

        //         sh "aws cloudformation wait stack-create-complete --stack-name ${env.STACK_NAME}"
        //     }
        // }
        stage('Setting up webapp with ansible') {
            steps {
                sh "ansible-playbook play.yaml -i aws_ec2.yaml --private-key ${SSH_KEY_CREDENTIAL}"
            }
        }
    }
}
