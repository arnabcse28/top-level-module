pipeline {
  agent any

  options {
    timeout(time: 15, unit: 'MINUTES')
    disableConcurrentBuilds()
  }

  stages {
    stage('build') {
      steps {
        script {
          withMaven(mavenSettingsConfig: 'local-nexus') {
            sh 'export PATH=$MVN_CMD_DIR:$PATH' + "&& mvn clean source:jar deploy -U -Dmaven.test.skip=true"
          }
        }
      }
    }
  }

  post {
    failure {
      emailext attachLog: false, body: """FAILED: Job Check console output at '${env.BUILD_URL}';""",
      recipientProviders: [[$class: 'CulpritsRecipientProvider'], [$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']], subject: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
    }
  }
}
