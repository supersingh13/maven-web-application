node{

def mavenHome = tool name: "maven 3.8.5"

echo "Node name is : {$env.NODE_NAME}"
echo "Job name is : {$env.JOB_NAME}"
echo "Build Number is : {$env.BUILD_NUMBER}"

//Checkout Code
stage('CheckoutCode'){
  git branch: 'development', credentialsId: 'edb77690-20e8-4757-9518-53dde672e7be', url: 'https://github.com/supersingh13/maven-web-application.git'
}


//Build Code
stage('Build'){
  sh "$mavenHome/bin/mvn clean package"
}

//SonarQube
stage('SonarQube Coverage'){
  sh "$mavenHome/bin/mvn sonar:sonar"
}

//NexusDeploy
stage('Deploy to Nexus'){
  sh "$mavenHome/bin/mvn deploy"
}

//DeployTomcat
stage('Deploy to Tomcat'){
	sshagent(['4901b1fd-48c5-4d6b-b655-f0d5894358f7']){
		sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@3.91.230.219:/opt/apache-tomcat-9.0.59/webapps"

	}
}

}
