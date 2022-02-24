node('nodes'){

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])])


def mavenHome = tool name: "Maven-3.8.4"

stage('checkoutcode')
{
git branch: 'development', credentialsId: 'e5092f01-30b9-41e4-b10c-7751b9d2cdcd', url: 'https://github.com/vinod-devops2/maven-web-application.git'
}

stage('Build')
{
sh  "${mavenHome}/bin/mvn clean package"
}

stage('SonarReport')
{
sh "${mavenHome}/bin/mvn sonar:sonar"
}

stage('upload to artifactory')
{
sh "${mavenHome}/bin/mvn deploy"
}

stage ('deploy to tomcat server')
{
sshagent(['b240bfdc-04bb-4d64-94f1-fdd02f431cc8'])
{
  sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.127.208.183:/opt/apache-tomcat-9.0.56/webapps"
}
} 

}
