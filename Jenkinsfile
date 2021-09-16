pipeline {
    agent any
	tools {
	    maven "3.6.3"
	 	}
	stages {
	    stage('Git CheckOut') {
		    steps {
			   git branch: 'main', credentialsId: '', url: 'https://github.com/logicopslab/WebAppForJenkins.git'
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
                 url: 'http://52.15.132.83:8082/artifactory',
                 username: 'admin',
                  password: 'Password1',
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
                      "target": "maven101-libs-snapshot-local"
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
