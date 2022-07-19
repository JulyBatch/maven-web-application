node {

def mavenHome = tool name: "maven3.8.6"

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: ''))])

echo "the Job name is: ${env.JOB_NAME}"
echo "the Node name is: ${env.NODE_NAME}"
echo "the build number is: ${env.BUILD_NUMBER}"
echo "the workspace path is: ${env.WORKSPACE}"
echo "the node label is: ${env.NODE_LABELS}"


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


