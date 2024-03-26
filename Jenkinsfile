pipeline{
    agent any
    stages{
        stage("Maven Build"){
           steps{
               sh 'mvn package'
           } 
        }
           stage("Upload Artifacts"){
            steps{
               script{
                    def pom = readMavenPom file: 'pom.xml'
                    def version = pom.version
                    def repoName = version.endsWith("SNAPSHOT") ? "do-snapshot": "do-release"
                    nexusArtifactUploader artifacts: [[artifactId: 'doctor-online', classifier: '', file: 'target/doctor-online.war', type: 'war']], 
                        credentialsId: 'nexuus3', 
                        groupId: 'in.javahome', 
                        nexusUrl: '172.31.45.174:8081', 
                        nexusVersion: 'nexus3', 
                        protocol: 'http', 
                        repository: repoName, 
                        version: version
               }
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
                  echo "umma"
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


