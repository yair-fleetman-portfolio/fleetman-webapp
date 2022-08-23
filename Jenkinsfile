pipeline {
   agent any
   
   tools { maven 'Maven' }

   environment {
     SERVICE_NAME = "webapp"
     REPOSITORY_TAG="644435390668.dkr.ecr.eu-central-1.amazonaws.com/yair-fleetman"
   }

   stages {

      stage('Build') {
         steps {
            echo '====================================='
            echo 'BUILD'
            echo '====================================='
            sh 'mvn clean package'
         }
      }

      stage('Build and Push Image') {
         steps {
            script {
               sh """#!/bin/bash
                  aws ecr get-login-password --region eu-central-1 | \
                  docker login --username AWS --password-stdin 644435390668.dkr.ecr.eu-central-1.amazonaws.com
                  docker build -t ${REPOSITORY_TAG}:${SERVICE_NAME}-1.0 .
                  docker push ${REPOSITORY_TAG}:${SERVICE_NAME}-1.0
                  """
            }
         }
      }

      // stage('Deploy to Cluster') {
      //     steps {
      //               sh 'envsubst < ${WORKSPACE}/deploy.yaml | kubectl apply -f -'
      //     }
      // }
   }

   post {
    always {
      cleanWs()
    }
  }
}
