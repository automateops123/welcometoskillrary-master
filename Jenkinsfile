pipeline {
	agent{label "welcometoskillrary"}
	tool {
		maven 'maven'
	}
	
		stages {
		stage (Clean Workspace) {
			steps {
				dir('java-source'){
            				sh "mvn package"
				}
			}
		}
	}
	
	stages {
		stage (SCM Checkout) {
			steps {
				git credentialsId: 'praveen-github', url: 'https://github.com/automateops123/welcometoskillrary-master.git'
			}
		}
	}

	
}
