pipeline {
  agent any
  stages {
    stage('BUILD') {
      steps {
        bat 'gradle build'
        bat 'gradle javadoc'
        archiveArtifacts 'build/libs/**.jar'
        archiveArtifacts 'build/reports/*'
      }
    }

  }
}