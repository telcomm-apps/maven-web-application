node {
def mavenHome = tool name: "maven3.8.6"
echo "the job name is : ${env.JOB_NAME}"
echo "the node name is : ${env.NODE_NAME}"
echo "the jenkins hme directory is : ${env.JENKINS_HOME}"
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
 try{
    sendSlackNotifications('STARTED')
stage('CheckoutCode'){
git credentialsId: 'e368c193-068f-4853-af28-232e8e8d7473', url: 'https://github.com/telcomm-apps/maven-web-application.git'
}
stage('Build'){
sh "${mavenHome}/bin/mvn clean package"
}
/*stage('ExecuteSonarQubeReport'){
sh "${mavenHome}/bin/mvn sonar:sonar"
}
stage('UploadArtifactsIntoNexus'){
sh "${mavenHome}/bin/mvn deploy"
}
stage('DeploAppIntoTomcatServer'){
sshagent(['11790cdf-134d-48ad-b75b-f7422a12ee16']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.33.174:/opt/apache-tomcat-9.0.70/webapps/"
}
}*/
}//try end
catch(e){
  currentBuild.result = "FAILURE"
  }
finally{
  sendSlackNotifications(currentBuild.result)
}
}
//slack notification
def sendSlackNotifications(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus =  buildStatus ?: 'SUCCESS'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    colorName = 'ORANGE'
    colorCode = '#FFA500'
  } else if (buildStatus == 'SUCCESS') {
    colorName = 'GREEN'
    colorCode = '#00FF00'
  } else {
    colorName = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend (color: colorCode, message: summary,channel: '#icicibank')
}
