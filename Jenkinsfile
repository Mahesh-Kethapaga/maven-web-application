 node{
	 
	 
	  //def mavenHome = tool(/var/lib/jenkins/tools/hudson.tasks.Maven_MavenInstallation/Maven3.9.6/) 
	  //==>is going to give path here only but mvn command available in the /bin/ directory
	  	
	  def mavenHome = tool name:"Maven3.9.6"
	
	 //Getting the code or Chechout stage	
	 stage('CheckoutCode'){
	 	// By default it is going to take the code from master branch. other than master branch we need to specify the branc name
		
		git branch: 'development', credentialsId: '6cfaa081-e697-448a-8bd0-22101123e699', url: 
		'https://github.com/Mahesh-Kethapaga/maven-web-application.git'

	}
	
	//Build stage	
	 stage('Build'){
	 
		//mvn clean package => directly we are not going to execute these commands becz it's linux server so use "sh"
		
		sh "$mavenHome/bin/mvn clean package" // For Linux and Mac
		
		//bat "mvn clean package" // For Windows		
		
	}
	
	//Generate SonarQube Report
	 stage('SonarQubeReport'){
	 
	 	// Before executing this command 1st you should integrate sonarqube server details in the pom.xml          
		sh "$mavenHome/bin/mvn sonar:sonar"	
	}
	
	//Upload Artifact Into Artifact Repo
	 stage('UploadArtifactIntoNexus'){
	 
	 	// Before executing this command 1st you should configure Nexus repo details         
		sh "$mavenHome/bin/mvn deploy"	
	}
	
	//Deploy App Into Tomcat Server
	 stage('DeployAppIntoTomcat'){
	 
	//Deploying application into the Tomcat Server is nothing but copying this .war file into the webapps folder where tomcat installed
		// To Copy file from one server to another server we use "SCP" Command and this command ask target server credentials
		//Manually we are not able to give so we generate some token nd we use that token so for that one we are going to use
		//SSHAgent plugin
		//For Tomcat server we have username but don't have passwd for that we use Pem file , we also connet using pem file also
		
		sshagent(['792eacc6-e53e-43e2-8815-0d1c20b2bb2e']) {
    		sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@54.213.56.98:/opt/tomcat/webapps"
		}
	}
	 
}//Node Closing
