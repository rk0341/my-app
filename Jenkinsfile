pipeline {
  agent any

    stages {
    stage('Maven Build') {
      steps {
        sh 'mvn clean package'
      }
    }
    stage('Deploy to Tomcat') {
      steps {
       sshagent(['tomcat-dev2']) {
           // copy war file to tomcat server
            sh 'scp -o StrictHostKeyChecking=no target/*.war ec2-user@172.31.47.249:/opt/tomcat8/webapps/app.war'
            // stop tomcat
            sh "ssh ec2-user@172.31.47.249 /opt/tomcat8/bin/shutdown.sh"
            // start tomcat
             sh "ssh ec2-user@172.31.47.249 /opt/tomcat8/bin/startup.sh"
            
    
        }
      }
    }
  }
  post {
    success {
        archiveArtifacts artifacts: 'target/*.war'
        }
        // Clean after build
        always {
            cleanWs(cleanWhenNotBuilt: false,
                    deleteDirs: true,
                    disableDeferredWipeout: true,
                    notFailBuild: true,
                    patterns: [[pattern: '.gitignore', type: 'INCLUDE'],
                               [pattern: '.propsfile', type: 'EXCLUDE']])
        }
    }
  
}
