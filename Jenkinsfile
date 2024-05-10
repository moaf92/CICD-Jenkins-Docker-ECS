pipeline {
	agent any
	environment {
		registryCredential = 'ecr:us-east-1:awscreds'
		appRegistry = "211125432472.dkr.ecr.us-east-1.amazonaws.com/projectmoafimg"
		projectRegistry = "https://211125432472.dkr.ecr.us-east-1.amazonaws.com"
		cluster = "vproject"
		service = "projectsvc"
	}

	stages {
	    stage('Fetch Code') {
	       steps{
	         git branch: 'main', url: 'https://github.com/moaf92/CICD-Jenkins-Docker-ECS.git'
	       }
	    }

	    stage('Test') {
	       steps{
	          sh 'mvn test'
	       }
	    } 

	    stage('Code Analyse With Checkstyle') {
	       steps{
	          sh 'mvn checkstyle:checkstyle'
	       }
	       post {
	          success {
	            echo 'Generate Analysis Result '
	           
	          }
	       }
	    }   

	    stage('sonar Analysis') {
	       environment {
	          scannerHome = tool 'sonar4.7'
	       }
	       steps {
               withSonarQubeEnv('sonar') {
                   sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=project \
                   -Dsonar.projectName=project \
                   -Dsonar.projectVersion=1.0 \
                   -Dsonar.sources=src/ \
                   -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                   -Dsonar.junit.reportsPath=target/surefire-reports/ \
                   -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                   -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
              }
	       }
	    }

	    stage("Quality Gate") {
         steps {
              timeout(time: 1, unit: 'HOURS') {
                waitForQualityGate abortPipeline: true
              }
           }
        }

      stage('Build App Image') {
       steps {
       
         script {
                dockerImage = docker.build( appRegistry + ":$BUILD_NUMBER", "./Dockerfile/")
      		}
      	}
      }
	   stage('Upload App Image') {
	     steps {
	     	 script {
	     	 	docker.withRegistry( projectRegistry, registryCredential ) {
	     	 		dockerImage.push("$BUILD_NUMBER")
	     	 		dockerImage.push('latest')
	     	 	}
	     	 }
	     }
	    stage('Deploy to ECS') {
          steps {
        withAWS(credentials: 'awscreds', region: 'us-east-1') {
          sh 'aws ecs update-service --cluster ${cluster} --service ${service} --force-new-deployment'
        }
      }
     }

    }

  }   
