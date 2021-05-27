node{
	stage('SCM Checkout'){

		git 'https://github.com/automateops123/welcometoskillrary-master.git'
	}
	
  	stage('Copile-Package'){
		// Get maven home path 
		def mvnHome = tool name: 'maven', type: 'maven'
		sh "${mvnHome}/bin/mvn clean package"
	}
  	
	stage('Junit Testing Reports'){

		junit '**/target/surefire-reports/*.xml'
	}


	stage('Jacoco Reports'){

		jacoco deltaBranchCoverage: '10'
	}
  
}
