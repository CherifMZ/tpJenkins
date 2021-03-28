pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        bat 'C:/gradle-5.6/bin/gradle build'
        bat 'C:/gradle-5.6/bin/gradle javadoc'
        archiveArtifacts 'build/libs/*.jar'
        archiveArtifacts 'build/docs/javadoc/*'
        junit(allowEmptyResults: true, testResults: 'build/test-results/test/*.xml')
      }
    }

    stage('Mail Notification') {
      steps {
        mail(subject: 'Mail Notifications', body: 'Jenkins', cc: 'hs_boucheta@esi.dz', to: 'hm_latreche@esi.dz')
      }
    }

    stage('Code Analysis') {
      steps {
        withSonarQubeEnv('sonar ') {
          bat 'C:/gradle-5.6/bin/gradle sonarqube'
        }

        waitForQualityGate true
      }
    }

  }
}