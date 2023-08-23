pipeline {
  agent any
  environment {
    CLOUDSDK_CORE_PROJECT='provana-395314'
    CLIENT_EMAIL='240781568858-compute@developer.gserviceaccount.com'
    GCLOUD_CREDS=credentials('gcloud-creds')
    DOCKERHUB_CREDENTIALS = credentials('docker')
  	}
  stages {
	    stage('Verify version') {
	      steps {
		sh '''
		  gcloud version
		'''
	      }
	    }
	    stage('Authenticate') {
	      steps {
		sh '''
		  gcloud auth activate-service-account --key-file="$GCLOUD_CREDS"
		'''
	      }
	    }
	    stage('Login') {
			    steps {
		      sh "echo $DOCKERHUB_CREDENTIALS_USR"
		   sh "echo $DOCKERHUB_CREDENTIALS_PSW"
				sh  "echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR -p $DOCKERHUB_CREDENTIALS_PSW"
				}
			}
	    stage('Unzip File') {
            steps {
                sh 'unzip -o python.zip'
pipeline {
  agent any
  environment {
    CLOUDSDK_CORE_PROJECT='provana-395314'
    CLIENT_EMAIL='240781568858-compute@developer.gserviceaccount.com'
    GCLOUD_CREDS=credentials('gcloud-creds')
    DOCKERHUB_CREDENTIALS = credentials('docker')
  	}
  stages {
	    stage('Verify version') {
	      steps {
		sh '''
		  gcloud version
		'''
	      }
	    }
	    stage('Authenticate') {
	      steps {
		sh '''
		  gcloud auth activate-service-account --key-file="$GCLOUD_CREDS"
		'''
	      }
	    }
	    stage('Login') {
			    steps {
		      sh "echo $DOCKERHUB_CREDENTIALS_USR"
		   sh "echo $DOCKERHUB_CREDENTIALS_PSW"
				sh  "echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR -p $DOCKERHUB_CREDENTIALS_PSW"
				}
			}
	    stage('Unzip File') {
            steps {
                sh 'unzip -o python.zip'
                sh 'rm -f python.zip'
            }
        }
		
	    
	  stage('build') {
	      steps {
		sh '''
		  docker build -t moredatta574/python-runner .
		'''
	      }
	    }
	  stage('push') {
	      steps {
		sh '''
		  docker push moredatta574/python-runner
		'''
	      }
	    }
	  
	    stage('pull'){
		    steps{
		       sh "docker pull  moredatta574/python-runner"
		    }
		 }
	       stage('tag'){
		    steps{
		       sh "docker tag  moredatta574/python-runner jenkins-demo gcr.io/provana-395314/moredatta574python-runner"
		    }
		 }
	    
	     stage('list'){
		    steps{
		       sh "gcloud container images list --repository=gcr.io/provana-395314"
		    }
		 }


	    stage('Install service') {
	      steps {
		sh '''
		  gcloud run services replace service.yaml --platform='managed' --region='asia-south1'
		'''
	      }
	    }
	    stage('Allow allUsers') {
	      steps {
		sh '''
		  gcloud run services add-iam-policy-binding hello-php --region='asia-south1' --member='allUsers' --role='roles/run.invoker'
		'''
	      }
	    }
  }
  post {
    always {
      sh 'gcloud auth revoke $CLIENT_EMAIL'
    }
  }
}

            }
        }
		
	    
	  stage('build') {
	      steps {
		sh '''
		  docker build -t moredatta574/python-runner .
		'''
	      }
	    }
	  stage('push') {
	      steps {
		sh '''
		  docker push moredatta574/python-runner
		'''
	      }
	    }
	  
	    stage('pull'){
		    steps{
		       sh "docker pull  moredatta574/python-runner"
		    }
		 }
	       stage('tag'){
		    steps{
		       sh "docker tag  moredatta574/python-runner jenkins-demo gcr.io/provana-395314/moredatta574python-runner"
		    }
		 }
	    
	     stage('list'){
		    steps{
		       sh "gcloud container images list --repository=gcr.io/provana-395314"
		    }
		 }


	    stage('Install service') {
	      steps {
		sh '''
		  gcloud run services replace service.yaml --platform='managed' --region='asia-south1'
		'''
	      }
	    }
	    stage('Allow allUsers') {
	      steps {
		sh '''
		  gcloud run services add-iam-policy-binding hello-php --region='asia-south1' --member='allUsers' --role='roles/run.invoker'
		'''
	      }
	    }
  }
  post {
    always {
      sh 'gcloud auth revoke $CLIENT_EMAIL'
    }
  }
}
