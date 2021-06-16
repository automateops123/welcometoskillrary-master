pipeline {
	agent{label "Maven Project"}
	
	
	
	stages {
		stage (Clean Workspace) {
			steps {
				def mvnHome = tool name: 'maven', type: 'maven'
				sh "${mvnHome}/bin/mvn clean "
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
