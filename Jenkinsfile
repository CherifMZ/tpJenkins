pipeline {
  agent any
  environment {
        email = ""
    }
  stages {
    stage('build') {
      steps {
        bat 'C:/gradle-5.6/bin/gradle build'
        bat 'C:/gradle-5.6/bin/gradle javadoc'
        archiveArtifacts 'build/libs/*.jar'
        archiveArtifacts 'build/docs/javadoc/*'
        junit(allowEmptyResults: true, testResults: 'build/test-results/test/*.xml')
      }
      
      post {
        success {
          script {
            email = "Jenkins Success"
          }
        }
        
        failure {
          script {
            email = "Jenkins Fail"
          }
        }
      }
    }

    stage('Mail Notification') {
      steps {
        mail(subject: 'Mail Notifications', body: email, cc: 'hs_boucheta@esi.dz', to: 'hm_latreche@esi.dz')
      }
    }

    stage('Code Analysis') {
      parallel {
        stage('Code Analysis') {
          steps {
            withSonarQubeEnv('sonar') {
              bat(script: 'C:/gradle-5.6/bin/gradle sonarqube', returnStatus: true)
            }

            waitForQualityGate true
          }
        }

        stage('Test Reporting') {
          steps {
            cucumber 'reports/*.json'
          }
        }

      }
    }

    stage('Deployment') {
      steps {
        bat 'C:/gradle-5.6/bin/gradle publish'
      }
    }

    stage('Slack Notification') {
      steps {
        slackSend(baseUrl: 'https://hooks.slack.com/services/', token: 'T01SFD57G30/B01T0QYTRB3/17KbduWNreCJNOwSV3av7aES', channel: '#tp-jenkins', teamDomain: 'jenkinsequipe.slack.com', message: 'notification : API is deployed')
      }
    }

  }
}
