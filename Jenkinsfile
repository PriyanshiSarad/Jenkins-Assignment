pipeline{
	agent any
	tools{
        jdk "JAVA8"
        maven "MAVEN3.8"
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
	    
	}
}
