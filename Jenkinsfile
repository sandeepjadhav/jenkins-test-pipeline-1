#!/usr/bin/env
pipeline {
  agent any
  environment {
    NODE_VERSION = '14.18.1'
    SERVER_ID = 'nodeAppID'
    ARTIFCTORY_URL: 'https://sandeepjadhav.jfrog.io/artifactory'
    ARTIFCTORY_USER_NAME: 'admin'
    ARTIFCTORY_PASSWORD: 'Password@123'
  }
  stages {
    stage('Install dependencies') {
      steps {
        bat "npm install"
      }
    }
    
    stage('Remove Old Build and Create new') {
            steps {
                // bat 'rm -rf *.tar.gz *'
                bat 'del *.tar.gz'
                bat 'tar czf my_build.tar.gz *'
            }
        }
      
     stage('Artifactory configuration') {
            steps {
                rtServer(
                        id: SERVER_ID,
                           url: ARTIFCTORY_URL,
                      // If you're using username and password:
                      username: ARTIFCTORY_USER_NAME,
                      password: ARTIFCTORY_PASSWORD,
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
                                    "pattern": "${env.WORKSPACE}/my_build.tar.gz",
                                    "target": "nodejenkinapp/",
                                    "recursive": "false"
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
