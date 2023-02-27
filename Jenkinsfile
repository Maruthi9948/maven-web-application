node{
properties([[$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
echo "branchname is: ${env.BRANCH_NAME}"
echo "jenkins home is: ${env.JENKINS_HOME}"
echo "jenkins url is: ${env.JENKINS_URL}"
def mavenhome = tool name: "maven3.8.5"
stage ('checkoutcode'){
git branch: 'development', credentialsId: '38e0e02c-c5d0-415e-a2f0-8f2fb433566e', url: 'https://github.com/Maruthi9948/maven-web-application.git'
    
}
stage( 'build'){
sh "${mavenhome}/bin/mvn clean package"
}
/* stage ( 'execute sonarqubereport'){
withSonarQubeEnv('SonarQube9.6.1'){
sh "${mavenhome}/bin/mvn sonar:sonar"
}
stage ( 'artifactsreponexus'){
sh "${mavenhome}/bin/mvn deploy"
}
stage( 'deployintotomcatserver'){
sshagent(['d6ac356e-8c18-48db-b56e-e53b6f32fec7']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@43.205.140.11:/opt/apache-tomcat-9.0.71/webapps/"
}
}
}*/
}
def slack(String buildStatus = 'STARTED') {
    // Build status of null means success.
    buildStatus = buildStatus ?: 'SUCCESS'

    def color

    if (buildStatus == 'STARTED') {
        color = '#D4DADF'
    } else if (buildStatus == 'SUCCESS') {
        color = '#BDFFC3'
    } else if (buildStatus == 'UNSTABLE') {
        color = '#FFFE89'
    } else {
        color = '#FF9FA1'
    }

    def msg = "${buildStatus}: `${env.JOB_NAME}` #${env.BUILD_NUMBER}:\n${env.BUILD_URL}"

    slackSend(color: color, message: msg)
}

node {
    try {
        notifySlack()

        // Existing build steps.
    } catch (e) {
        currentBuild.result = 'FAILURE'
        throw e
    } finally {
        slack(currentBuild.result)
    }
}
