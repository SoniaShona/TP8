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
      parallel {
        stage('Code Analysis') {
          steps {
            withSonarQubeEnv 'sonar'
            waitForQualityGate true
            powershell 'gradle sonarqube'
          }
        }

        stage('error') {
          steps {
            jacoco()
          }
        }

      }
    }

  }
}