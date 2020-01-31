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
      environment {
        PATH = 'D:\\sonar-scanner-cli-4.2.0.1873-windows\\sonar-scanner-4.2.0.1873-windows\\bin'
      }
      steps {
        withSonarQubeEnv 'sonar'
        waitForQualityGate true
        powershell 'gradle sonarqube'
      }
    }

  }
}