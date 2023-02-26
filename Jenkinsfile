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
}
}*/
