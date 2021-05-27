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
	
	stage('SonarQube Analysis'){ 
		def mvnHome = tool name: 'maven', type: 'maven'
		withSonarQubeEnv('sonarquber'){
    		// some block
		sh "${mvnHome}/bin/mvn sonar:sonar -Dsonar.projectKey=welcometoskillrary -Dsonar.host.url=http://13.233.72.168:9000 -Dsonar.login=b993f2e2516629fdb0997287eb8e93f15e915a26"
		}	
 
	}	
}
