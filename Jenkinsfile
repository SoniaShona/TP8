pipeline {
  agent any
  stages {
    stage('BUILD') {
      steps {
        powershell 'gradle build'
        powershell 'gardle javadoc'
        archiveArtifacts 'build/docs/**'
        archiveArtifacts 'build/libs/**.jar'
        archiveArtifacts 'build/reports/*'
      }
    }

  }
}