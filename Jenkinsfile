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
    stage('Checkout repo') {
      steps {
        checkout scm
      } 
    }

    stage('Install deps') {
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

    stage('terraform fmt') {
      steps {
        sh 'terraform fmt -list=true -write=false -diff=true -check=true -recursive'
      }
    }

    stage('terraform plan') {
      steps {
        sh 'terraform plan -no-color -out=.tfplan'
      }
    }

        // Wait for approval
    stage('approval') {
      // options {
      //    timeout(time: 1, unit: 'HOURS')
      // }
        steps {
          input 'approve the plan to proceed and apply'
      //notifySlack("Do you approve the Terraform plan? $jenkins_server_url/jenkins/job/$JOB_NAME", notification_channel, [])
        }
    }
  //stage('terraform apply') {
  //    steps {
  //       sh 'terraform apply'
  //     }
  //}
  // stage('Clean up workspace') {
  //     steps {
  //       cleanWs()
  //     }
  //   }
  // }
  }
  post {
        always {
            echo 'Clean up woprkspace'
            cleanWs() /* clean up our workspace */
        }
        success {
            echo 'Succeeded'
            mail to: 'polina.apostolova@ecs-digital.com',
             subject: "Pipeline Succeeded: ${currentBuild.fullDisplayName}",
             body: "Something is wrong with ${env.BUILD_URL}"
        }
        unstable {
            echo 'Unstable :/'
        }
        failure {
            echo 'Failed'
        }
        changed {
            echo 'Changes applied'
        }
    }
}
