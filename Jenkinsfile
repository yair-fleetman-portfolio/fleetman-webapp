pipeline {
   agent any
   
   tools { maven 'Maven' }

   environment {
     SERVICE_NAME = "webapp"
     REPOSITORY_TAG="644435390668.dkr.ecr.eu-central-1.amazonaws.com/yair-fleetman"
   }

   stages {
      stage('Build Image') {
         when { expression { env.GIT_BRANCH == 'master' } }
         steps {
            script {
               sh "docker build -t ${REPOSITORY_TAG}:${SERVICE_NAME}-${BUILD_NUMBER} ."
            }
         }
      }


      stage ('test') {
         steps {
            echo 'No Tests Yet'
         }
      }      

      stage('Trigger update manifests') {
         when { expression { env.GIT_BRANCH == 'master' } }
         steps {
            script {
               echo '====================================='
               echo 'Triggering update manifest job'
               echo '====================================='
               build job: 'update-fleetman-manifest', parameters: 
               [
                  string(name: 'SERVICE', value: env.SERVICE_NAME),
                  string(name: 'IMAGE_TAG', value: env.BUILD_NUMBER),
                  string(name: 'GIT_COMMITTER_NAME', value: env.GIT_COMMITTER_NAME),
                  string(name: 'GIT_COMMITTER_EMAIL', value: env.GIT_COMMITTER_EMAIL)
               ]
            }
         }
      }      
   }

   post {
    always {
      cleanWs()
    }
  }
}
