pipeline {
    agent any
	tools {
	    maven "3.6.3"
	 	}
	stages {
	    stage('Git CheckOut') {
		    steps {
			   git branch: 'main', credentialsId: 'My private token login creds', url: 'https://github.com/logicopslab/WebAppForJenkins.git'
			}
		}
        stage('Clean and Install') {
            steps {
                bat 'mvn clean install'
            }
        }
        stage ('Package'){
            steps {
                bat 'mvn package'
             }
        }
	stage ('Server'){
            steps {
               rtServer (
                 id: "Artifactory",
                 url: 'http://localhost:8082/artifactory',
                 username: 'ravish',
                  password: 'YouPasswordHere',
                  bypassProxy: true,
                   timeout: 300
                        )
            }
        }
        stage('Upload'){
            steps{
                rtUpload (
                 serverId:"Artifactory" ,
                  spec: '''{
                   "files": [
                      {
                      "pattern": "*.war",
                      "target": "logic-ops-lab-libs-snapshot-local"
                      }
                            ]
                           }''',
                        )
            }
        }
        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "Artifactory"
                )
            }
        }
    }
}
