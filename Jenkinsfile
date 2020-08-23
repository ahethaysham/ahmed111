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
		  stage('Package' ) {
                    steps{
			echo "Package"
			sh "mvn package -DskipTests "    
						 }
						 }   
		 stage('Build Docker Image') {
                    steps{
			echo "Build Docker Image"
			script {    
			//sh "docker build -t abe0282/jenkins:$env.BUILD_TAG"
			dockerimage = docker.build("abe0282/jenkins:${env.BUILD_TAG}")   
						 }
						 } 
		 }
		  stage('Push Docker Image') {
                    steps{
			echo "Build Docker Image"
			script {  
			docker.withRegistry('','dockerhub'){	
			dockerimage.push();
			//dockerimage.push(latest);
			}
						 }
						 } 
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
