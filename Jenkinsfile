pipeline{
    agent any
    tools{
        terraform 'terraform'
    }
    stages{
        stage('Git checkout'){
            steps{
                git branch: 'main', url: 'https://github.com/rickyticky2/Java_deploy_Jenkins-terraform_ansible.git'
            }
        }
        stage('Create Instances with terraform'){
            steps{
                    sh 'sudo terraform init'
                    sh 'sudo terraform apply -auto-approve'
                    script{
                        IP1 = sh (
                        script: 'terraform output first_ip',
                        returnStdout: true
                        ).trim()
                        IP2 = sh (
                        script: 'terraform output second_ip',
                        returnStdout: true
                        ).trim()
                    }
              }
        }
        stage ('Run ansible playbook'){
            steps{
                    withCredentials([usernamePassword(credentialsId: 'xxxxxxxxxxxxxxx', passwordVariable: 'DPWD', usernameVariable: 'DUSER')]) {

                        ansiblePlaybook disableHostKeyChecking: true, installation: 'ansible', playbook: 'awsb.yml', become: true, extras: "-e'IP1=${IP1}' -e'IP2=${IP2}' -e'USER=${USER}' -e'PWD=${PWD}'"
                    }
              }
         }
    }
}