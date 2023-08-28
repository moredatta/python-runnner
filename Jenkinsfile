pipeline {
    agent any
    environment {
        CLOUDSDK_CORE_PROJECT = 'provana-395314'
        CLIENT_EMAIL = 'python-source@provana-395314.iam.gserviceaccount.com'
        GCLOUD_CREDS = credentials('gcloud-creds')
        DOCKERHUB_CREDENTIALS = credentials('docker')
        SERVICE_NAME = "tunner"
        REGION = "us-central1"
        IMAGE_NAME = "python-runner1"
        REGISTRY_HOSTNAME = "asia.gcr.io"
    }
    stages {
        stage('Authenticate') {
            steps {
                withCredentials([file(credentialsId: 'gcloud-creds', variable: 'GOOGLE_APPLICATION_CREDENTIALS')]) {
                    sh "gcloud auth activate-service-account --key-file=${GOOGLE_APPLICATION_CREDENTIALS}"
                    sh "gcloud auth configure-docker"
                }
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
                dir('python') { // Change to the actual path
                    sh 'docker build -t python-runner1 .'
                }
            }
        }

        stage('Tag Docker Image') {
            steps {
                sh 'docker tag python-runner1 ${REGISTRY_HOSTNAME}/${CLOUDSDK_CORE_PROJECT}/${IMAGE_NAME}'
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push ${REGISTRY_HOSTNAME}/${CLOUDSDK_CORE_PROJECT}/${IMAGE_NAME}'
            }
        }

        stage('List Images') {
            steps {
                sh "gcloud container images list --repository=${REGISTRY_HOSTNAME}/${CLOUDSDK_CORE_PROJECT}"
            }
        }

        stage('Deploy to Cloud Run') {
            steps {
                sh '''
                    gcloud run deploy ${SERVICE_NAME} \
                    --image=${REGISTRY_HOSTNAME}/${CLOUDSDK_CORE_PROJECT}/${IMAGE_NAME} \
                    --platform=managed \
                    --region=${REGION} \
                    --project=${CLOUDSDK_CORE_PROJECT} \
                    --allow-unauthenticated
                '''
            }
        }
    }
    post {
        always {
            sh "gcloud auth revoke ${CLIENT_EMAIL}"
        }
    }
}
