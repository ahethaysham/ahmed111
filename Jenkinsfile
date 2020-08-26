pipeline {
   agent any
   //agent { docker { image 'maven:3.6.3'} }
   //agent { docker { image 'node:13.8'} } 	
   //agent {
   //     docker {
   //         image 'maven:3-alpine' 
   //         args '-v /root/.m2:/root/.m2' 
   //	}}
	environment {
		dockerHome = tool 'myDocker'
		mavenHome  = tool 'myMaven'
		PATH       = "$dockerHome/bin:$mavenHome/bin:$PATH" }
            stages {
                stage('Checkout') {
                    steps{
			sh 'mvn --version'
			sh 'docker version'    
			echo "Build"
			echo "PATH - $PATH"
			echo "BUILDNUMBER -$env.BUILD_NUMBER"
			echo "BUILD_ID -$env.BUILD_ID"
			echo "JOB_NAME -$env.JOB_NAME"
			echo "BUILD_TAG -$env.BUILD_TAG"
			echo "BUILD_URL -$env.BUILD_URL"    
		    }
						 }
                stage('Compile') {
                    steps{
			echo "Compile"
			sh "mvn clean compile"    
						 }
						 }
		     stage('Test') {
                    steps{
			echo "Test for maven"
			sh "mvn test"    
						 }
						 }
                stage('Integration Test') {
                    steps{
			echo "Integration Test"
			sh "mvn failsafe:integration-test failsafe:verify "    
						 }
						 }
		stage('Build') {
    			sh 'mvn clean package'
    			withCredentials([usernamePassword(credentialsId: env.ACR_CRED_ID, usernameVariable: 'ACR_USER', passwordVariable: 'ACR_PASSWORD')]{
      			sh 'docker login -u $ACR_USER -p $ACR_PASSWORD https://$ACR_SERVER'
      			// build image
      			def imageWithTag = "$env.ACR_SERVER/$env.WEB_APP:$env.BUILD_NUMBER"
      			def image = docker.build imageWithTag
    					}
		 			}
post {
		always {
			echo 'Im awesome. I run always'
		}
		success {
			echo 'I run when you are successful'
		}
		failure {
			echo 'I run when you fail'
		}
	}
}
