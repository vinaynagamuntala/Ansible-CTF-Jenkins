pipeline {
    agent any
    
    // tools {
    //     terraform'terraform'
    //     ansible'ansible'
    // }

    environment {
        AWS_DEFAULT_REGION = "us-east-1"
        AWS_CONFIGURE = credentials('AWS_Creds')
        STACK_NAME = "myEc2Stack"
    }

    stages {
        stage('git checkout') {
            steps{
                sh "rm -rf Ansible-CTF-Jenkins"
                sh "git clone https://github.com/vinaynagamuntala/Ansible-CTF-Jenkins.git"
                //sh "cd Ansible-CTF-Jenkins"
                //sh "git pull origin main"

            }
        }
        stage('aws setup Check') {
            steps {
                sh 'aws sts get-caller-identity'
            }
        }
        // stage('CloudFormation stack Provision') {
        //     steps {

        //         sh "aws cloudformation create-stack --stack-name ${env.STACK_NAME} --template-body file://app.yaml"
        //     }
        // }
        // stage('Wait for CloudFormation Stack Creation') {
        //     steps {

        //         sh "aws cloudformation wait stack-create-complete --stack-name ${env.STACK_NAME}"
        //     }
        // }
        stage('Configure web app with ansible') {
            withEnv(['SSH_KEY_CREDENTIAL = credentials(\'Private_key\')']) {
                //steps {
                //withCredentials([sshUserPrivateKey(credentialsId: 'Private_key')])
                sh "ansible-playbook play.yaml -i aws_ec2.yaml --private-key ${SSH_KEY_CREDENTIAL}"
                //}
            }

        }

    }
}
