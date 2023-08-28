pipeline {
    agent any
    environment {
        CLOUDSDK_CORE_PROJECT = 'provana-395314'
        CLIENT_EMAIL = 'python-source@provana-395314.iam.gserviceaccount.com'
        GCLOUD_CREDS = credentials('gcloud-creds')
        DOCKERHUB_CREDENTIALS = credentials('docker')
	SERVICE_NAME = "tunner"
        REGION = "us-central1"
        //IMAGE_NAME = "python-runnerr"
        REGISTRY_HOSTNAME = "asia.gcr.io"
      
    }
    stages {
        stage('Authenticate') {
         steps {
           withCredentials([file(credentialsId: 'gcloud-creds', variable: 'GOOGLE_APPLICATION_CREDENTIALS')]) {
         
           sh 'gcloud auth activate-service-account --key-file=${GOOGLE_APPLICATION_CREDENTIALS}'
           
           sh  'gcloud auth configure-docker --quiet'
          // sh  'gcloud auth configure-docker asia.gcr.io'
         
            }
        }
        } 
        stage('Login to DockerHub') {
            steps {
                sh "echo $DOCKERHUB_CREDENTIALS_USR"
                sh "echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin"
            }
        }

	//stage('Clone Code from Google Source Repository') {
        //    steps {
         //       sh 'git clone https://github.com/moredatta/python-runnner'
         //   }
    //    }
     

        stage('Unzip File') {
            steps {
                sh 'unzip -o python.zip'
                sh 'rm -f python.zip'
            }
        }
      
     
       stage('Build Docker Image') {
            steps {
                dir('python') { // Change to the actual path
                    sh 'docker build -t python-runner1 .'
                }
            }
        }

        
        stage('tag Docker Image') {
            steps {
                sh 'docker tag  python-runner1 gcr.io/provana-395314/python-runner1'
            }
        }
        stage('Push Docker Image') {
            steps {
                sh 'docker push gcr.io/provana-395314/python-runner1'
            }
        }
        
        stage('List Images') {
            steps {
                sh 'gcloud container images list --repository=gcr.io/provana-395314'
            }
        }
        
        //stage('Deploy Cloud Run Service') {
           // steps {
        //        sh "gcloud run deploy python-runner --image=gcr.io/provana-395314/python-runner1 --platform=managed  --port=8080 --region=us-east-1"
          //  }
     //   }

	 stage('Deploy to Cloud Run') {
      steps {
        // Deploy the Docker image to Cloud Run
sh '
          gcloud run deploy ${SERVICE_NAME} \
            --image=${REGISTRY_HOSTNAME}/${PROJECT_ID}/${IMAGE_NAME} \
            --platform=managed \
            --region=${REGION} \
            --project="provana-395314"
            --allow-unauthenticated
        '
      }
    }   
    }
    post {
        always {
            sh 'gcloud auth revoke $CLIENT_EMAIL'
        }
    }
}
