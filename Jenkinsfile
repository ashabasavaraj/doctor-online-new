@Library("jhc-libs") _

pipeline {
    agent any

    stages {
        stage('Maven Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage("develop branch"){
           when{ branch 'develop'  }
        step{
                
        echo "develop branch"
    }
        }
         stage("test branch"){
           when{ branch 'test'  }
        step{
                
        echo "test branch"
    }
        } 
        stage("main branch"){
           when{ branch 'main'  }
        step{
                
        echo "main branch"
    
        }
    }
}    
        post {
      success {
        cleanWs()
      }
    }
}
