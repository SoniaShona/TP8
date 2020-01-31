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

    stage('test report') {
      parallel {
        stage('test report') {
          steps {
            jacoco()
            powershell 'gradle jacocoTestCoverageVerification'
          }
        }

        stage('Code analysis') {
          steps {
            withSonarQubeEnv 'SonarQube'
            waitForQualityGate true
            powershell 'gradle sonarqube'
          }
        }

      }
    }

    stage('Depoly') {
      steps {
        powershell 'gradle publishing'
      }
    }

  }
}