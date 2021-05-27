node{
	stage('SCM Checkout'){

		git 'https://github.com/automateops123/welcometoskillrary-master.git'
	}
	
  	stage('Copile-Package'){
		// Get maven home path 
		def mvnHome = tool name: 'maven', type: 'maven'
		sh "${mvnHome}/bin/mvn clean package"
	}
  
  
}
