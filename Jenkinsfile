node{

	stage('clean the workspace'){
		sh "$/usr/share/apache-maven/bin/mvn clean"	
	}	
	
	stage('SCM Checkout'){
		git 'https://github.com/automateops123/welcometoskillrary-master.git'
	}
	
	stage('compile the code'){
		sh "$/usr/share/apache-maven/bin/mvn clean compile"
	}
	
	stage('SonarQube Analysis'){
		withSonarQubeEnv(credentialsId: 'jenkins-integration', installationName: 'sonarqube') {
			sh "/usr/share/apache-maven/bin/mvn sonar:sonar"
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
	
  	stage('Packaging the code'){
		sh "$/usr/share/apache-maven/bin/mvn clean package"
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
