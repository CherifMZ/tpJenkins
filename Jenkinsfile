pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        bat 'C:/gradle-5.6/bin/gradle build'
        bat 'C:/gradle-5.6/bin/gradle javadoc'
        archiveArtifacts 'build/libs/*.jar'
      }
    }

  }
}