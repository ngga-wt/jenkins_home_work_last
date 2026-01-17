
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
                  echo "You choice is: ${params.LANGUGE}"
                  if( "${params.LANGUGE}" == 'all' ){
                    fileName = sh(script: "find ${PUB_DIR}/ -type f | tr '\n' ' '", returnStdout: true).trim()
                    fileContent = sh(script: "cat $fileName", returnStdout: true).trim()
                  }else {
                    fileName = "${params.LANGUGE}.txt"
                    fileContent = sh(script: "cat $PUB_DIR/$fileName", returnStdout: true).trim()
                  }
                  echo "$fileContent"
              }
            }
        }
    }
    post {
      always {
        echo "send mail with choosen file name\\s $fileName"
      }
      // success {
      //   DO SOMETHING
      // }
      // failure {
      //   mail to: team@example.com, subject: 'The Pipeline ${PROJECT_NAME} failed :('
      // }
    }
}
