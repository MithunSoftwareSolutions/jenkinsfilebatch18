node
{

properties([[$class: 'JiraProjectProperty'], buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])])

def mavenHome = tool name: "maven3.6.2"


stage('CheckoutCode')
{
  git branch: 'development', credentialsId: 'df465b4e-8599-49b0-a2d4-eed216745582', url: 'https://github.com/MithunTechnologiesDevOps/maven-web-application.git'
}

stage('Build')
{
 
 sh "${mavenHome}/bin/mvn clean package"
}

stage('SonarQubeReportExecution')
{
sh "${mavenHome}/bin/mvn sonar:sonar"
}

stage('UploadArtifactIntoNexus')
{
sh "${mavenHome}/bin/mvn deploy"
}

stage('DeployAppintoTomcatServer')
{
 sshagent(['b1483976-b2e8-4c04-83d1-0bc255c511d2']) 
 {
   sh "scp -o StrictHostKeyChecking=no  target/maven-web-application.war ec2-user@52.66.211.45:/opt/apache-tomcat-9.0.36/webapps/"
 }
}

stage('EmailNotification')
{
mail bcc: 'devopstrainingblr@gmail.com', body: '''Build Over.

Regards,
Mithun Technologies,
9980923226''', cc: 'devopstrainingblr@gmail.com', from: '', replyTo: '', subject: 'Build over', to: 'devopstrainingblr@gmail.com'
}

}
