node {
def mavenHome = tool name: "maven3.8.6"
echo "the job name is : ${env.JOB_NAME}"
echo "the node name is : ${env.NODE_NAME}"
echo "the jenkins hme directory is : ${env.JENKINS_HOME}"
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
stage('CheckoutCode'){
git branch: 'development', credentialsId: 'ae0b84b1-cb2d-4a71-894e-297682504fd7', url: 'https://github.com/telcomm-apps/maven-web-application.git'
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
}
