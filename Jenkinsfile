pipeline{
	agent any
	tools{
        jdk "JAVA8"
        maven "MAVEN3.8"
	}
	environment{
		APPLICATION_NAME = 'Jenkins-Java-App'
		VERSION_LABEL = 'jenkins-java-app-source'
		BUCKET_NAME = 'java-artifacts-0101'
		ARTIFACT_NAME = 'ROOT.war'
	}
	stages{
	    stage("Fetch code from GitHub Repo"){
	        steps{
	            git branch:'master', url:'https://github.com/ravdy/hello-world.git'
	        }
	    }
	    stage("Build using Maven"){
	        steps{
	            sh 'mvn clean install'
	        }
	        post{
	           success{
	                 echo "Archiving Artifact"
	                 archiveArtifacts artifacts: '**/*.war'
	           }
	        }
	    }
	    stage("Deploy to Elastic Beanstalk"){
	    	steps{
	    		withAWS(credentials: 'awsCredentials', region: 'us-east-1'){
                    sh 'aws s3 cp ./webapp/target/webapp.war s3://java-artifacts-0101/ROOT.war'
	           }
	    	}
	    }	    
	}
}
