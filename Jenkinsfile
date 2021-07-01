pipeline {
  environment {
    registry = "javaajisaiharsha/calculator"
    registryCredential = 'dockerhub'
    if [[$GIT_BRANCH == "dev"]]
    then
        KUBECONFIG="$JENKINS_HOME/.kube/config1"
    elif [[$GIT_BRANCH == "dev"]]
    then
        KUBECONFIG="$JENKINS_HOME/.kube/config2"
    fi
  }
  agent any
  stages {
    stage('Building image') {
      steps{
        sh "printenv"
        
        sh "docker build -t javaajisaiharsha/calculator:$BUILD_ID-$BRANCH_NAME ." 
       // sh "docker run -dp 80:80 javaajisaiharsha/calculator-search:$BUILD_ID"
        sh "docker push javaajisaiharsha/calculator$BUILD_ID-$BRANCH_NAME"
      }
    }
    stage('Creating Deployment') {
      steps {
    
        sh  '''#!/bin/bash
                
                if [[ $GIT_BRANCH == "dev" ]]
                then
                     kubectl set image deployment/calc-app nginx=javaajisaiharsha/calculator:$BUILD_ID-$BRANCH_NAME -n $BRANCH_NAME
                elif [[ $GIT_BRANCH == "master" ]]
                then
                    kubectl set image deployment/calc-app nginx=javaajisaiharsha/calculator:$BUILD_ID-$BRANCH_NAME -n prod
                fi         
            '''
      }
    }
    stage('') {
      steps{
        sh '''
            kubectl get deployments -n $BRANCH_NAME
            kubectl get svc -n $BRANCH_NAME
            '''
      }
    }
    }
}
