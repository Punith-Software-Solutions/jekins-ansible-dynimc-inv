

pipeline {
  agent { 
  label 'ansible'
  }
  environment {
   AWS_EC2_PRIVATE_KEY=credentials('ec2-private-key') 
  }
  
  stages {
    
    //Get the Code from GitHub Repo
    stage('CheckOutCode'){
      steps{
        git credentialsId: '5b9436f3-1052-49e0-b090-8060667f8fe6', url: 'https://github.com/Punith-Software-Solutions/jenkins-with-ansible.git'
      }
    }
     
    //Using Terrafrom can create the Servers
  /*  
    stage('CreateServers'){
      steps{
       sh "terraform  -chdir=terraformscripts init"
       sh "terraform  -chdir=terraformscripts apply --auto-approve"
      }
    }
    */
    
    //Run the playbook
    stage('RunPlaybook') {
      steps {
        sh "whoami"
        //List the dymaic inventory just for verification
        sh "ansible-inventory --graph -i inventory/aws_ec2.yaml"
        //Run playbook using dynamic inventory & limit exuection only fo tomcatservers.
        sh "ansible-playbook -i inventory/aws_ec2.yaml  playbooks/tomcat-setup.yaml -u ec2-user --private-key=$AWS_EC2_PRIVATE_KEY --limit tomcatservers --ssh-common-args='-o StrictHostKeyChecking=no'"
      }
    }
  
  }//stages closing
}//pipeline closing
