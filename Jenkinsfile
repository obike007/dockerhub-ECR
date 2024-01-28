pipeline {
  agent any 

  environment {
    SERVICE_NAME        = "container-registry"
    ORGANIZATION_NAME   = "obike007"
    DOCKERHUB_USERNAME  = "obike007"
    REGISTRY_TAG        = "${DOCKERHUB_USERNAME}/${ORGANISATION_NAME}-${SERVICE_NAME}:${BUILD_ID}"
  }


  
  stages {
    
    stage ('Print ENVs') {
      steps {
        sh 'printenv'
      }
    }
      
    stage ('Build Image') {
      steps {
        withDockerRegistry([credentialsId: 'docker-hub', url: ""]) {
          sh 'docker build -t ${REGISTRY_TAG} .'
          sh 'docker push ${REGISTRY_TAG}'
        }
      } 
    } 
    stage ('Delete') {
      steps {
        sh 'docker rmi -f $(docker images -qa)'
      }
    }
    
  }
} 
