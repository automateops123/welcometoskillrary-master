node('master'){

	stage('SCM Checkout'){
		git 'https://github.com/automateops123/welcometoskillrary-master.git'
	}
	
	
	stage('clean the workspace'){
		sh "/usr/share/apache-maven/bin/mvn clean"	
	}
	
	stage('Compile the code'){
		sh "/usr/share/apache-maven/bin/mvn compile"
	}
	
	stage('Source Code Analysis'){
		withSonarQubeEnv('sonarqube1'){
		sh "/usr/share/apache-maven/bin/mvn sonar:sonar -Dsonar.projectKey=jenkins-integration -Dsonar.host.url=http://175.41.183.57:9000 -Dsonar.login=e15564d90c10691e253a9441e48bbf31ba0e0343"
		}
	}
	
        stage("Quality Gate"){
          	timeout(time: 1, unit: 'HOURS') {
              		def qg = waitForQualityGate()
              		if (qg.status != 'OK') {
                  	error "Pipeline aborted due to quality gate failure: ${qg.status}"
                }
            }
         }  
	
  	stage('Packaging the code'){
		sh "/usr/share/apache-maven/bin/mvn package"
	}	
	
	stage('Junit Testing Reports'){
		junit '**/target/surefire-reports/*.xml'
	}

	stage('jacoco reports'){
          	jacoco(
                	execPattern: '**/target/jacoco.exec',
                	classPattern: '**/coverage/**',
                	sourcePattern: '**/coverage/**',
                	inclusionPattern: '**/*.class'
			)
   	}
	

}
