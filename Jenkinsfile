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

	stage('jacoco reports'){
          jacoco(
                execPattern: '**/target/jacoco.exec',
                classPattern: '**/coverage/**',
                sourcePattern: '**/coverage/**',
                inclusionPattern: '**/*.class'
		)
      }
	
	stage('SonarQube Analysis'){
		withSonarQubeEnv(credentialsId: 'jenkins-integration', installationName: 'sonarqube') {
			sh "${mvnHome}/bin/mvn sonar:sonar"
		}
	}	
	
	 stage('Quality Gate'){
         	 timeout(time: 1, unit: 'HOURS') {
              		def qg = waitForQualityGate()
              		if (qg.status != 'OK') {
                  	error "Pipeline aborted due to quality gate failure: ${qg.status}"
              }
          }
      }
	
	stage('Jfrog Artifactory Backup'){
	
		def server = Artifactory.server "sonarqube"
		// Create an Artifactory Maven instance.
		def rtMaven = Artifactory.newMavenBuild()
		def buildInfo
		// Tool name from Jenkins configuration
       		rtMaven.tool = "maven"
        	// Set Artifactory repositories for dependencies resolution and artifacts deployment.
        	rtMaven.deployer releaseRepo:'skillrarywelcome-libs-release-local', snapshotRepo:'skillrarywelcome-libs-snapshot-local', server: server
        	rtMaven.resolver releaseRepo:'skillrarywelcome-libs-release-local', snapshotRepo:'skillrarywelcome-libs-snapshot-local', server: server

	}
	stage('Deploy to Tomcat Server'){

		deploy adapters: [tomcat8(credentialsId: 'Tomcat-Jenkins', path: '', url: 'http://54.254.75.108:9090')], contextPath: 'skillrary01', war: '**/*.war'
	}		
}
