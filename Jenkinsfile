pipeline {
  agent any
  stages {
    stage('BUILD') {
      steps {
        powershell 'gradle build'
        powershell 'gradle javadoc'
        archiveArtifacts 'build/libs/**.jar'
        archiveArtifacts 'build/reports/*'
      }
    }

  }
}