
def PUB_DIR = 'public'
def fileName = ''
def fileContent = ''

pipeline {
  agent any
  environment {
      START_TIME = sh(script: 'date +"%Y-%m-%d %H:%M:%S"',returnStdout: true).trim()
      USER_NAME = sh(script: 'whoami', returnStdout: true).trim()
  }
  parameters {
    choice(name: 'LANGUGE', choices: ['c', 'java', 'python', 'all'], description: 'Select languge')
  }
  stages {
    stage('Start stage') {
      steps {
        script {
          // Print Environment variables
          echo "START_TIME: [$START_TIME] ,BUILD_NUMBER: $BUILD_NUMBER ,JOB_NAME: $JOB_NAME AND USER_NAME: ${USER_NAME}"
          if( "${params.LANGUGE}" == 'all' ){
            fileName = sh(script: "find ${PUB_DIR}/ -type f | tr '\n' ' '", returnStdout: true).trim()
          }else {
            fileName = "$PUB_DIR/${params.LANGUGE}.txt"
          }
          fileContent = sh(script: "cat $fileName", returnStdout: true).trim()
          echo "$fileContent"
        }
      }
    }
  }
  post {
    always {
      wrap([$class: 'BuildUser']) {
        script {
          def userEmail = env.BUILD_USER_EMAIL
          echo "Build triggered by: ${env.BUILD_USER}"
          echo "User Email: ${userEmail}"
          emailext (
            to: "${userEmail}",
            subject: "Jenkins Build Status: ${currentBuild.currentResult}",
            body: """
              <html>
                <body>
                    <span>$START_TIME</span>
                    <h2>Build Notification</h2>
                    <h4>Selected Languge\\s is: ${params.LANGUGE} Languge\\s</h4>
                    <p>Project: ${env.JOB_NAME}</p>
                    <p>Build Number: ${env.BUILD_NUMBER}</p>
                    <hr />
                    <p>
                      Content of choosen file\\s:
                      <pre>$fileContent</pre>
                    </p>
                    <hr />
                    <p>Status: <b>${currentBuild.currentResult}</b></p>
                    <p>Check console output at: <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                </body>
              </html>
            """.stripIndent(),
            mimeType: 'text/html'
          )
        }
      }
    }
  }
}
