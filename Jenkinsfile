pipeline {
  agent any
  stages {
    stage('BUILD') {
      steps {
        powershell 'gradle build'
        powershell 'gradle javadoc'
        archiveArtifacts 'build/libs/**.jar'
        archiveArtifacts 'build/reports/**'
      }
    }

    stage('Mail Notification') {
      steps {
        mail(subject: 'Build Result', body: 'the build has been done', to: 'gk_benaida@esi.dz')
      }
    }

    stage('Code Analysis') {
      agent any
      steps {
        withSonarQubeEnv 'sonarQube'
        waitForQualityGate true
        powershell 'gradle sonarqube'
      }
    }

  }
}