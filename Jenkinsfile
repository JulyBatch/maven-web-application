try {

node {

def mavenHome = tool name: "maven3.8.6"

stage('Checkout_code'){
  git branch: 'development', credentialsId: '9297fbac-023e-4149-a033-eb803760ba6a', url:
  'https://github.com/JulyBatch/maven-web-application.git'
  }
  
stage('Build'){
sh "${mavenHome}/bin/mvn clean package"

}

stage('SonarQubeReport'){
sh "${mavenHome}/bin/mvn clean package sonar:sonar"
}

stage('UploadArtifact')
{
sh "${mavenHome}/bin/mvn deploy"
}

stage('DeployApplicationIntoTomcatServer')
{
 sshagent(['11776570-b386-4784-8453-ffeb673e31e2']) {
 sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@65.1.148.193:/opt/apache-tomcat-9.0.64/webapps/"
    
}

}
}
catch(e){
 currentBuild.result = 'Failed'
 throw e
 }
 finally{
 //success or failure send the notifications
 slacknotifications(currentBuild.result)
 }

def slacknotifications(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus =  buildStatus ?: 'SUCCESSFUL'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'YELLOW'	
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESSFUL') {
    color = 'GREEN'
    colorCode = '#00FF00'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend (color: colorCode, message: summary)
}
