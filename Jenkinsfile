pipeline{
	agent any
	tools{
        jdk "JAVA8"
        maven "MAVEN3.8"
	}
	environment{
		APPLICATION_NAME = 'Jenkins-Java-App'
		ENV_NAME = 'Java-env-Prod'
		VERSION_LABEL = "${BUILD_ID}"
		BUCKET_NAME = 'java-artifacts-0101'
		ARTIFACT_NAME = 'ROOT.war'
	}
	stages{
	    stage("Fetch code from GitHub Repo"){
	        steps{
	            git branch:'master', url:'https://github.com/PriyanshiSarad/Jenkins-Assignment.git'
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
                    sh 'aws elasticbeanstalk create-application-version --application-name ${APPLICATION_NAME} --version-label ${VERSION_LABEL} --source-bundle S3Bucket=${BUCKET_NAME},S3Key=${ARTIFACT_NAME} '
                    sh 'aws elasticbeanstalk update-environment --application-name ${APPLICATION_NAME} --environment-name ${ENV_NAME} --version-label ${VERSION_LABEL} '
	           }
	    	}
	    }	    
	}
}
