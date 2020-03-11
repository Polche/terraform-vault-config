env.terraform_version = '0.12.18'

pipeline {
  agent {
    node {
      label 'master'
    }
  }
    // agent {
    //     docker {
    //      image 'hashicorp/terraform:light'
    //     }
    // }
      stages {
        stage('install_deps') {
          steps {
            sh "apt install wget zip python-pip -y"
            sh "apt-get update && apt-get install -y"
            sh "cd /tmp"
            sh "curl -o terraform.zip https://releases.hashicorp.com/terraform/'$terraform_version'/terraform_'$terraform_version'_linux_amd64.zip"
            sh "unzip terraform.zip"
            sh "mv terraform /usr/bin"
            sh "rm -rf terraform.zip"
          }
        }

        stage('terraform init') {
          steps {
            sh 'terraform init'
          }
        }
        // Run terraform fmt and validate

        // stage('terraform fmt') {
        //   steps {
        //     sh 'terraform fmt ...'
        //   }
        // }
        stage('terraform plan') {
          steps {
            sh 'terraform plan -no-color -out=.tfplan'
          }
        }
        // Wait for approval
        //     stage('approval') {
        //     //   options {
        //     //     timeout(time: 1, unit: 'HOURS')
        //     //   }
        //      steps {
        //         input 'approve the plan to proceed and apply'
        //       }
        //     stage('terraform apply') {
        //       steps {
        //         sh 'terraform apply'
        //       }
        //     }
        //   }
      }
    }
