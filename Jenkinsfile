pipeline{
    agent any
    stages{
        stage("Maven Build"){
           steps{
               sh 'mvn package'
           } 
        }
        stage("Dev Deploy"){
           steps{
              sshagent(['tomcat-dev']) {
                // Copy war file to tomcat dev server
                sh "scp -o StrictHostKeyChecking=no target/doctor-online.war ec2-user@172.31.20.35:/opt/tomcat/webapps/"
                // Restart tomcat server
                sh "ssh ec2-user@172.31.20.35 /opt/tomcat/bin/shutdown.sh"
                sh "ssh ec2-user@172.31.20.35 /opt/tomcat/bin/startup.sh"
                  echo "hi jhc"
              }
           } 
        }
    }

post {
  cleanup {
    cleanWs()
  }
}
}


