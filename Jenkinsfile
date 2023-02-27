node{
properties([[$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
echo "branchname is: ${env.BRANCH_NAME}"
echo "jenkins home is: ${env.JENKINS_HOME}"
echo "jenkins url is: ${env.JENKINS_URL}"
def mavenhome = tool name: "maven3.8.5"
   try {
      notifyBuild('STARTED')
stage ('checkoutcode'){
git branch: 'development', credentialsId: '38e0e02c-c5d0-415e-a2f0-8f2fb433566e', url: 'https://github.com/Maruthi9948/maven-web-application.git'
    
}
stage ( 'build'){
sh "${mavenhome}/bin/mvn clean package"
}
/* stage ( 'execute sonarqubereport'){
withSonarQubeEnv('SonarQube9.6.1'){
sh "${mavenhome}/bin/mvn sonar:sonar"
}
stage ('artifactsreponexus'){
sh "${mavenhome}/bin/mvn deploy"
}
stage( 'deployintotomcatserver'){
sshagent(['d6ac356e-8c18-48db-b56e-e53b6f32fec7']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@43.205.140.11:/opt/apache-tomcat-9.0.71/webapps/"
}
}
*/

 } 
catch (e) {
    // If there was an exception thrown, the build failed
    currentBuild.result = "FAILURE"
    throw e
  } finally {
    // Success or failure, always send notifications
    notifyBuild(currentBuild.result)
  }
}

def notifyBuild(String buildStatus = 'STARTED') {
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
