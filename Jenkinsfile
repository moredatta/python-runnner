pipeline {
    agent any
    environment {
        CLOUDSDK_CORE_PROJECT = 'provana-395314'
        CLIENT_EMAIL = '240781568858-compute@developer.gserviceaccount.com'
        GCLOUD_CREDS = credentials('gcloud-creds')
        DOCKERHUB_CREDENTIALS = credentials('docker')
      
    }
    stages {
        stage('Authenticate') {
            steps {
                sh 'gcloud auth activate-service-account --key-file="$GCLOUD_CREDS"'
            }
        }
        stage('Login to DockerHub') {
            steps {
                sh "echo $DOCKERHUB_CREDENTIALS_USR"
                sh "echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin"
            }
        }
     

        stage('Unzip File') {
            steps {
                sh 'unzip -o python.zip'
                sh 'rm -f python.zip'
            }
        }
        stage('Build Docker Image') {
            steps {
                // Change to the actual path
                    sh 'docker build -t moredatta574/python-runner .'
                
            }
        }
        stage('Push Docker Image') {
            steps {
                sh 'docker push moredatta574/python-runner'
            }
        }
        stage('Pull Docker Image') {
            steps {
                sh 'docker pull moredatta574/python-runner'
            }
        }
        stage('Tag Docker Image') {
            steps {
                sh 'docker tag moredatta574/python-runner gcr.io/provana-395314/moredatta574python-runner'
            }
        }
        stage('List Images') {
            steps {
                sh 'gcloud container images list --repository=gcr.io/provana-395314'
            }
        }
        stage('Install Service') {
            steps {
                sh 'gcloud run services replace service.yaml --platform=managed --region=asia-south1'
            }
        }
        stage('Allow allUsers') {
            steps {
                sh 'gcloud run services add-iam-policy-binding hello-php --region=asia-south1 --member=allUsers --role=roles/run.invoker'
            }
        }
    }
    post {
        always {
            sh 'gcloud auth revoke $CLIENT_EMAIL'
        }
    }
}
