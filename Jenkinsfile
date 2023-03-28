pipeline {
    agent any
	tools {
		Maven '3.8.7' 
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
                 id: "artifactory-center",
                 url: 'http://20.0.186.254:8082/artifactory',
                 username: 'vladyslav',
                  password: 'Aa_4815162342',
                  bypassProxy: true,
                   timeout: 300
                        )
            }
        }
        stage('Upload'){
            steps{
                rtUpload (
                 serverId:"artifactory-center" ,
                  spec: '''{
                   "files": [
                      {
                      "pattern": "*.war",
                      "target": "test-artifactory-libs-snapshot-local"
                      }
                            ]
                           }''',
                        )
            }
        }
        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "artifactory-center"
                )
            }
        }
    }
}
