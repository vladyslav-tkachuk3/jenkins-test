pipeline {
	agent any

	stages {
		stage('git checkout') {
			steps {
				git branch: 'artifactory', credentialsId: 'github-credentials', url: 'https://github.com/vladyslav-tkachuk3/jenkins-test.git'
			}
		}
        	stage('clean and install') {
			steps {
                		sh 'mvn clean install'
			}
		}
		stage('package'){
			steps {
				sh 'mvn package'
			}
		}
		stage('server') {
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
		stage('upload') {
			steps {
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
		stage ('publish build info') {
			steps {
				rtPublishBuildInfo (
				serverId: "artifactory-center"
				)
			}
		}
	}
}
