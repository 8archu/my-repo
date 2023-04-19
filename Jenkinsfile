pipeline {
    agent any
    stages {
        stage('clean_workspace') {
            steps {
                // You can choose to clean workspace before build as follows
                cleanWs deleteDirs: true, notFailBuild: true
                checkout scm
               
            }
        }
        
        stage ('Build') {
            steps { 
                    sh 'mvn clean install -DskipTests=true '                                    
               }
         }
	    stage ('docker image build') {
		 environment
			{
				aws_access_key = credentials('aws-access-key')
				secret_key = credentials('secret-access-key')
                  }

             steps {
                     sh 'aws configure set aws_access_key_id $aws_access_key'
		     sh 'aws configure set aws_secret_access_key $aws_secret_key'
		     sh 'aws configure set default.region us-east-1'
		     sh 'aws ecr get-login-password --region ap-south-1 | sudo docker login --username AWS --password-stdin 800872446136.dkr.ecr.ap-south-1.amazonaws.com'
                     sh 'sudo docker build -t archu_123'
                     sh 'sudo docker tag archu_123:latest 800872446136.dkr.ecr.ap-south-1.amazonaws.com/archu_123:latest'
                     sh 'sudo docker push 800872446136.dkr.ecr.ap-south-1.amazonaws.com/archu_123:latest'
  		     
             }    
                    
        }
        

                   
    }
}
