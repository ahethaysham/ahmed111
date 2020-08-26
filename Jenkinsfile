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
			 stage('Build') {
			 steps {
    			sh 'mvn clean package'
    		  acrQuickBuild azureCredentialsId: '2a9b8c43-0a5c-4e9f-8b9b-7201fe52037b',
                  resourceGroupName: $env.aksrg1,
                  registryName: $env.acrahmed812,
                  platform: "Linux",
                  dockerfile: "Dockerfile",
                  imageNames: [[image: "$env.ACR_REGISTRY/$env.IMAGE_NAME:$env.BUILD_NUMBER"]]
    					}
		 			}
		    stage('DISTRIBUTE') {
  			//environment {
    			//APPCENTER_API_TOKEN = "bb294418c4a6a643d4af6ddaf7618b23a7c06752"
  			//			}
  		  steps {
			  sh "mkdir -p output"
			  writeFile file: "output/usefulfile.txt", text: "This file is useful, need to archive it."
			  archiveArtifacts artifacts: 'output/*'
			  appCenter apiToken: "bb294418c4a6a643d4af6ddaf7618b23a7c06752", appName: 'hvhhh', buildVersion: '', 
				  distributionGroups: 'testers', notifyTesters: false, ownerName: 'abe0282-gmail.com', 
				  pathToApp: 'output/*', pathToDebugSymbols: '', pathToReleaseNotes: '', releaseNotes: ''
		//appCenter apiToken: APPCENTER_API_TOKEN,
            	//ownerName: 'abe0282-gmail.com',
            	//appName: 'hvhhh',
            	//pathToApp: 'output/*',
            	//distributionGroups: 'casey, niccoli'
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
