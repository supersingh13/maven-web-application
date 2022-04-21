pipeline{

agent any
echo "THIS IS IN MASTER"
tools{
maven 'maven3.8.4'

}

triggers{
pollSCM('* * * * *')
}

options{
timestamps()
buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5'))
}

stages{

  stage('CheckOutCode'){
    steps{
    git branch: 'master', credentialsId: 'edb77690-20e8-4757-9518-53dde672e7be', url: 'https://github.com/supersingh13/maven-web-application.git'
	
	}
  }
  
  stage('Build'){
  steps{
  sh  "mvn clean package"
  }
  }

 stage('ExecuteSonarQubeReport'){
  steps{
  sh  "mvn clean sonar:sonar"
  }
  }
  
  stage('UploadArtifactsIntoNexus'){
  steps{
  sh  "mvn clean deploy"
  }
  }
  
  //DeployTomcat
   stage('Deploy to Tomcat'){
   steps{
	sshagent(['4901b1fd-48c5-4d6b-b655-f0d5894358f7']){
	sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@54.160.9.156:/opt/apache-tomcat-9.0.59/webapps"
	}
      }
    }
 
}//Stages Closing

post{

 success{
 emailext to: 'sujeetpal.singh@gmail.com',
          subject: "MASTER: Pipeline Build is over .. Build # is ..${env.BUILD_NUMBER} and Build status is.. ${currentBuild.result}.",
          body: "MASTER: Pipeline Build is over .. Build # is ..${env.BUILD_NUMBER} and Build status is.. ${currentBuild.result}.",
          replyTo: 'sujeetpal.singh@gmail.com'
 }
 
 failure{
 emailext to: 'sujeetpal.singh@gmail.com',
          subject: "MASTER: Pipeline Build is over .. Build # is ..${env.BUILD_NUMBER} and Build status is.. ${currentBuild.result}.",
          body: "MASTER: Pipeline Build is over .. Build # is ..${env.BUILD_NUMBER} and Build status is.. ${currentBuild.result}.",
          replyTo: 'sujeetpal.singh@gmail.com'
 }
 
}


}//Pipeline closing
