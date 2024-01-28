pipeline {
  agent any 

  environment {
    SERVICE_NAME        = "container-registry"
    ORGANIZATION_NAME   = "obike007"
    DOCKERHUB_USERNAME  = "obike007"
    REGISTRY_TAG        = "${DOCKERHUB_USERNAME}/${ORGANISATION_NAME}-${SERVICE_NAME}:${BUILD_ID}"
    REPO_TAG            = "public.ecr.aws/y9h6i0p3"
    APP_NAME            = "cohort5"
    VERSION             = "${BUILD_ID}"
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
    stage ('Publish to ECR') {
      steps {
        withEnv(["AWS_ACCESS_KEY_ID=${env.AWS_ACCESS_KEY_ID}", "AWS_SECRET_ACCESS_KEY}=${env.AWS_SECRET_ACCESS_KEY}", "AWS_DEFAULT_REGION=${env.AWS_DEFAULT_REGION}"]) {
          sh 'docker login -u AWS -p $(aws ecr-public get-login-password --region us-east-1) ${REPO_TAG}'
          sh 'docker build -t $(APP_NAME):$(VERSION) .'
          sh 'docker tag ${APP_NAME}:${VERSION} ${REPO_TAG}/${APP_NAME}:${VERSION}'
          sh 'docker push ${REPO_TAG}/${APP_NAME}:${VERSION}'
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
