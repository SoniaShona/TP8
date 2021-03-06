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
        mail(subject: 'Build Result', body: 'the build has been done', to: 'kaouthar.asma@gmail.com', from: 'gk_mahboubi@esi.dz', cc: 'gl_benaida@esi.dz', bcc: 'gl_benaida@esi.dz', replyTo: 'kaouthar.asma@gmail.com')
      }
    }

    stage('Code analysis') {
      parallel {
        stage('test report') {
          steps {
            jacoco(execPattern: 'build/jacoco/*.exec', exclusionPattern: '**/test/*.class')
          }
        }

        stage('Code Analysis') {
          steps {
            withSonarQubeEnv('sonar') {
              bat 'gradle sonarqube'
            }

            waitForQualityGate true
          }
        }

      }
    }

    stage('Depoly') {
      when {
        branch 'master'
      }
      steps {
        powershell 'gradle publishing'
      }
    }

    stage('slack notification') {
      when {
        branch 'master'
      }
      steps {
        slackSend(baseUrl: 'https://hooks.slack.com/services/', teamDomain: 'tp6-outil', token: 'TRQ5N6NJH/BRCM572RH/NxnK9GViNA6CDT0wVcota4N8')
      }
    }

  }
}