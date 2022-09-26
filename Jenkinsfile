#!/usr/bin/env
pipeline {
  agent any
  environment {
    NODE_VERSION = '14.18.1'
    SERVER_ID = 'nodeAppID'
  }
  stages {
    stage('Install dependencies') {
      steps {
        echo  "${WORKSPACE}"
      }
    }
    
      
     stage('Artifactory configuration') {
            steps {
                rtServer(
                        id: SERVER_ID,
                           url: 'https://sandeepjadhav.jfrog.io/artifactory',
                      // If you're using username and password:
                      username: 'admin',
                      password: 'Password@123',
                )
            }
        }
    
    
    stage ('Upload') {
            steps {
                rtUpload (
                    buildName: "${env.BUILD_NUMBER}",
                    buildNumber: "${env.BUILD_NUMBER}",
                    // Obtain an Artifactory server instance, defined in Jenkins --> Manage Jenkins --> Configure System:
                    serverId: SERVER_ID,
                  spec: """{
                                "files": [
                                    {
                                    "pattern": "${env.WORKSPACE}/(*)",
                                    "target": "nodejenkinapp/",
                                    "flat": "true",
                                    "recursive": "true"
                                    }
                                ]
                            }"""
                )
            }
        }
  
    

  }
  
  
  post {
    failure {
      echo 'Processing failed'
    }
    success {
      echo 'Processing succeeded'
    }
  }
}
