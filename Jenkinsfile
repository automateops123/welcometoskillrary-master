node('master'){

	stage('SCM Checkout'){
		git 'https://github.com/automateops123/welcometoskillrary-master.git'
	}
	
	
	stage('clean the workspace'){
		sh "/usr/share/apache-maven/bin/mvn clean"	
	}
}
