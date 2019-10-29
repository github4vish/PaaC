


// Powered by Infostretch 

timestamps {

node ('build') { 

	stage ('01 - Validate and Compile - Checkout') {
 	 checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', url: 'https://github.com/github4vish/spring-hibernate-maven-webapp.git']]]) 
	}
	stage ('01 - Validate and Compile - Build') {
 			// Maven build step
	withMaven { 
 			if(isUnix()) {
 				sh "mvn validate compile " 
			} else { 
 				bat "mvn validate compile " 
			} 
 		} 
	}
	stage ('02 - Unit Testing - Build') {
 			// Maven build step
	withMaven { 
 			if(isUnix()) {
 				sh "mvn test " 
			} else { 
 				bat "mvn test " 
			} 
 		}
		// JUnit Results
		junit '**/target/surefire-reports/*.xml' 
	}
}
node () { 

	stage ('03 - Cobertura Code Coverage - Build') {
 			// Maven build step
	withMaven { 
 			if(isUnix()) {
 				sh "mvn cobertura:cobertura -Dcobertura.report.format=xml " 
			} else { 
 				bat "mvn cobertura:cobertura -Dcobertura.report.format=xml " 
			} 
 		} 
	}
}
node ('build') { 

	stage ('04 - Documentation - Build') {
 			// Maven build step
	withMaven { 
 			if(isUnix()) {
 				sh "mvn javadoc:javadoc " 
			} else { 
 				bat "mvn javadoc:javadoc " 
			} 
 		} 
	}
	stage ('05 - Sonar Analysis - Code Analysis - Build') {
 
 environment {
        scannerHome = tool 'Sonar';
    //sonar.host.url='http://35.184.137.111:9000/':
    }
    
    withSonarQubeEnv('Sonar') {
            //sh "${scannerHome}/bin/sonar-scanner"
            sh "mvn clean compile sonar:sonar "
        }
    

    
// Unable to convert a build step referring to "hudson.plugins.sonar.SonarBuildWrapper". Please verify and convert manually if required.		// Maven build step
	//withMaven { 
 	//		if(isUnix()) {
 		    
 	//			sh "mvn clean compile sonar:sonar " 
	//		} else { 
 	//			bat "mvn clean compile sonar:sonar " 
	//		} 
 	//	} 
	}
}
node () { 

	stage ('06 - Packaging and Nexus Release - Build') {
 			// Maven build step
	withMaven(mavenSettingsFilePath: 'settings.xml') { 
 			if(isUnix()) {
 				sh "mvn clean package deploy " 
			} else { 
 				bat "mvn clean package deploy " 
			} 
 		} 
	}
}
node ('build') { 

	stage ('06.2 Verify Deployment - Build') {
 			// Ansible playbook 
 			sh "ansible-playbook /home/vish/tomcat/runsetup.yml -i 35.184.107.24,"
	}
	stage ('07 - Tomcat Deployment - Build') {
 			// Maven build step
	withMaven { 
 			if(isUnix()) {
 				sh "mvn clean package " 
			} else { 
 				bat "mvn clean package " 
			} 
 		} 
	}
}
}
