pipeline {
   //agent any
   //agent { docker { image 'maven:3.6.3'} }
   agent { docker { image 'node:13.8'} } 	
   //agent {
   //     docker {
   //         image 'maven:3-alpine' 
   //         args '-v /root/.m2:/root/.m2' 
   //	}}
            stages {
                stage('Build') {
                    steps{
			sh 'node --version'    
			echo "Build"
						 }
						 }
                stage('Test') {
                    steps{
			echo "Test"
						 }
						 }
                stage('Integration Test') {
                    steps{
			echo "Integration Test"
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
