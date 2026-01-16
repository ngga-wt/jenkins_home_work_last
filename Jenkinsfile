pipeline {
    agent any
    environment {
        START_TIME = sh(script: 'date +"%Y-%m-%d %H:%M:%S"',returnStdout: true).trim()
        USER_NAME = 'anonymous'
      }
    parameters {
      choice(name: 'LANGUGE', choices: ['c', 'java', 'python', 'all'], description: 'Select languge')
    }
    stages {
        stage('Start stage') {
            steps {
                // Print Environment variables
                echo "START_TIME: [$START_TIME] ,BUILD_NUMBER: $BUILD_NUMBER ,JOB_NAME: $JOB_NAME AND USER_NAME: ${USER_NAME}"
                echo "choice ${params.LANGUGE}"
            }
        }
    }
}
