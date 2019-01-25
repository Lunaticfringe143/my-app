  node{
   stage('SCM Checkout'){
     git 'https://github.com/Lunaticfringe143/my-app'
   }
   stage('Compile-Package'){
    
      def mvnHome =  tool name: 'maven-3', type: 'maven'   
      sh "${mvnHome}/bin/mvn package"
   }

    /*stage('SonarQube analysis') {
      
    withSonarQubeEnv('sonar-1') {

       def mvnHome = tool name: 'maven-3' , type: 'maven'

    // requires SonarQube Scanner for Maven 3.2+
      sh "${mvnHome}/bin/mvn sonar:sonar"

    }

  }*/
    stage('Analysis & Report') {
                 writeFile file: "${pwd()}/sonar-project.properties", text: """
                 #Mandatory meta data required
                 sonar.projectKey=sonarcheck
                 sonar.projectName=SonarDemo
                 sonar.projectVersion=1.0
                 #path to the src directory of the maven project
                 sonar.sources=src
                 sonar.jacoco.reportPath=target\\coverage-reports\\jacoco-unit.exec
                 #sonar.sources=src/main/java/
                 sonar.language=java
                 sonar.java.binaries=target/
                 """
                }
    stage('SonarQube analysis') {
      
    withSonarQubeEnv('sonar-1') {

       def mvnHome = tool name: 'maven-3' , type: 'maven'

    // requires SonarQube Scanner for Maven 3.2+
      sh "${mvnHome}/bin/mvn sonar:sonar"

    }

  }
    
    /*stage('Check Quality Gate') {        
                echo 'Checking quality gate...'
                timeout(time: 1, unit: 'HOURS') {
                        def swait = waitForQualityGate()
                        if (swait.status != 'OK') {
                            error "Pipeline aborted due to quality gate failure: ${swait.status}"
                        }
                 }
    }*/
     stage("Quality Gate Status Check"){
       echo 'Checking quality gate...'

          timeout(time: 1, unit: 'HOURS') {

              def qg = waitForQualityGate()

              if (qg.status != 'OK') {

                   /*slackSend baseUrl: 'https://hooks.slack.com/services/',

                   channel: '#jenkins-pipeline-demo',*/

                  /* color: 'danger', 

                   message: 'SonarQube Analysis Failed', 

                   teamDomain: 'gandhi',

                   tokenCredentialId: 'slack-demo'*/

                  error "Pipeline aborted due to quality gate failure: ${qg.status}"
              }


          }

      }    

  
   /*stage('Email Notification'){
      mail bcc: '', body: '''Hi Welcome to jenkins email alerts
      Thanks
      Hari''', cc: '', from: '', replyTo: '', subject: 'Jenkins Job', to: 'gandhi.bashyam@yahoo.com'
   }*/
   /*stage('Slack Notification'){
       slackSend baseUrl: 'https://hooks.slack.com/services/',
       channel: '#jenkins-pipeline-demo',
       color: 'good', 
       message: 'Welcome to Jenkins, Slack!', 
       teamDomain: '',
       tokenCredentialId: 'slack-demo'
   }*/
}


