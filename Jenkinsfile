pipeline {
   agent any

   environment {
     // You must set the following environment variables
     // ORGANIZATION_NAME
     // YOUR_DOCKERHUB_USERNAME (it doesn't matter if you don't have one)

     SERVICE_NAME="nodejsapp"
     ORGANIZATION_NAME="bhaskarsaini27"
     YOUR_DOCKERHUB_USERNAME="sainibha"
     REPOSITORY_TAG="${YOUR_DOCKERHUB_USERNAME}/nodejsapp-1.33:${BUILD_ID}"
   }

   stages {
      stage('Preparation') {
         steps {
            cleanWs()
            git credentialsId: 'GitHub', url: "https://github.com/${ORGANIZATION_NAME}/${SERVICE_NAME}"
         }
      }
    stage('Build Docker Image') {
            steps {
                script {
                  sh 'docker build -t ${REPOSITORY_TAG} .'
                }
            }
        }
    stage('Push Image to DockerHub') {
         steps {
           sh 'docker push ${REPOSITORY_TAG}'
         }
      }
    
    stage('Deploy App on k8s') {
      steps {
        withCredentials([
            string(credentialsId: 'my_kubernetes', variable: 'api_token')
            ]) {
             sh 'envsubst < ${WORKSPACE}/nodejsapp.yaml | kubectl --token $api_token --server https://192.168.49.2:8443  --insecure-skip-tls-verify=true apply -f -'
               }
            }
            }
        }
      
    }
