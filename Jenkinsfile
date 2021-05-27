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
		withSonarQubeEnv('sonarqube'){
		sh "${mvnHome}/bin/mvn sonar:sonar -Dsonar.projectKey=welcometoskillrary -Dsonar.host.url=http://13.233.72.168:9000 -Dsonar.login=b56fef816c2ed5d97e4d9ae26c94fd9cb57cdc22"
	}	
        stage("Quality Gate Check Status"){
         	 timeout(time: 1, unit: 'HOURS') {
             		 def qg = waitForQualityGate()
             		 if (qg.status != 'OK') {
                 		 error "Pipeline aborted due to quality gate failure: ${qg.status}"	
			 }
		 }
	}
}
