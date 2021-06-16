pipeline {
	any agent
	tool {
		def mvnHome = tool name: 'maven', type: 'maven'
	}
	stages {
		stage ( 'Clean Workspace' ) {
			steps {
				echo "Cleaning the Workspace"
				sh "${mvnHome}/bin/mvn clean "
			}
		}
	}
	stages {
		stage ( 'SCM Checkout' ) {
			steps {
				echo "Cloning the Repository"
				git credentialsId: 'praveen-github', url: 'https://github.com/automateops123/welcometoskillrary-master.git'
			}
		}
	}	
}
