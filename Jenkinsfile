  node{
   stage('SCM Checkout'){
     git 'https://github.com/Lunaticfringe143/my-app'
   }
   stage('Compile-Package'){
    
      def mvnHome =  tool name: 'maven-3', type: 'maven'   
      sh "${mvnHome}/bin/mvn package"
   }

    stage('SonarQube analysis') {
      
    withSonarQubeEnv('sonar-1') {

       def mvnHome = tool name: 'maven-3' , type: 'maven'

    // requires SonarQube Scanner for Maven 3.2+
      sh "${mvnHome}/bin/mvn sonar:sonar"

    }

  }
    
    stage('sonar-check'){
      timeout(time: 1, unit: 'HOURS'){
        def aa = waitForQualityGate()
        if(aa.status != 'OK'){
          error "Pipeline aborted due to check in quality gate: ${aa.status}"
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
       teamDomain: 'javahomecloud',
       tokenCredentialId: 'slack-demo'
   }*/
}


