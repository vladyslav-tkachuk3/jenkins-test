pipeline {
    agent any
	tools {
	    maven "3.8.7"
	 	}
	stages {
	    stage('Git CheckOut') {
		    steps {
			   git branch: 'artifactory', credentialsId: 'github-credentials', url: 'https://github.com/vladyslav-tkachuk3/jenkins-test.git'
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
