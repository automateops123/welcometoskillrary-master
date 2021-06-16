node{

	stage('clean the workspace'){
		def mvnHome = tool name: 'maven', type: 'maven'
		sh "${mvnHome}/bin/mvn clean"	
	}	
	
	stage('SCM Checkout'){
		git 'https://github.com/automateops123/welcometoskillrary-master.git'
	}
	
	stage('Compile the code'){
		def mvnHome = tool name: 'maven', type: 'maven'
		sh "${mvnHome}/bin/mvn clean compile"
	}
	
	stage('Source Code Analysis'){
		def mvnHome = tool name: 'maven', type: 'maven'
		withSonarQubeEnv('sonarqube1'){
		sh "${mvnHome}/bin/mvn sonar:sonar -Dsonar.projectKey=jenkins-integration -Dsonar.host.url=http://54.179.64.45:9000 -Dsonar.login=e15564d90c10691e253a9441e48bbf31ba0e0343"
		}
	}
	
	stage("Quality Gate"){
		no public field ‘webhookSecretId’ (or getter method) found in class org.sonarsource.scanner.jenkins.pipeline.WaitForQualityGateStep
      }  
	
  	stage('Packaging the code'){
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
	
	stage('Jfrog Artifactory Backup'){
	
		def server = Artifactory.server "sonarqube"
		def rtMaven = Artifactory.newMavenBuild()
		def buildInfo
       		rtMaven.tool = "maven"
        	rtMaven.deployer releaseRepo:'skillrarywelcome-libs-release-local', snapshotRepo:'skillrarywelcome-libs-snapshot-local', server: server
        	rtMaven.resolver releaseRepo:'skillrarywelcome-libs-release-local', snapshotRepo:'skillrarywelcome-libs-snapshot-local', server: server
	}
	stage('Deploy approval'){
		input "Deploy to prod?"
	}
	
	stage('Deploy to QAServer'){
		deploy adapters: [tomcat8(credentialsId: 'Tomcat-Jenkins', path: '', url: 'http://54.254.75.108:9090')], contextPath: 'skillrary01', war: '**/*.war'
	}
	    
	stage("slack channel"){
        	slackSend channel: '#automateops123', color: 'Blue', message: 'Job is Successfully Deployed QA server', teamDomain: 'automateopsworkspace', tokenCredentialId: 'slackintjenk'   
    }
}
