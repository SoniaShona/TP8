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

    stage('Code analysis') {
      parallel {
        stage('test report') {
          steps {
            jacoco()
            powershell 'gradle jacocoTestCoverageVerification'
          }
        }

        stage('Code Analysis') {
          steps {
            powershell 'gradle sonarqube'
          }
        }

      }
    }

    stage('Depoly') {
      when{
    tag "release-*"
  }
      steps {
        powershell 'gradle publishing'
      }
    }

    stage('slack notification') {
    
      steps {
        slackSend(baseUrl: 'https://hooks.slack.com/services/', teamDomain: 'tp6-outil', token: 'TRQ5N6NJH/BRCM572RH/NxnK9GViNA6CDT0wVcota4N8')
      }
    }

  }
}
