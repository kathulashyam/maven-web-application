node{
    
properties([[$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])

echo "The job name is: ${env.JOB_NAME}"
echo "The Nod ename is: ${env.NODE_NAME}"
echo "The Build Number is: ${env.BUILD_NUMBER}"
echo "The Jenkins Home directory is: ${env.JENKINS_HOME}"

def mavenHome = tool name: "maven3.8.5"

stage('CheckoutCode') {
git branch: 'development', credentialsId: 'dee84286-8890-48ef-9e82-778a874e474e', url: 'https://github.com/kathulashyam/maven-web-application.git'
}

stage ('Build') {
sh "${mavenHome}/bin/mvn clean package"
}

stage ('ExecuteSonarQubeReport') {
sh "${mavenHome}/bin/mvn sonar:sonar"
}

stage ('UploadArtifactsIntoNexus') {
sh "${mavenHome}/bin/mvn deploy"
}

stage ('DeployAppIntoTomcatServer') {
sshagent(['9911be94-6423-4f80-841c-3aa2dbe6bbb5']) {
  sh "scp -o  StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.40.228:/opt/apache-tomcat-9.0.70/webapps/"	
}
}

}
