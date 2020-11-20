pipeline {
  agent any
    environment {
    RELEASE = '20.11'
  }
  stages {
    stage('Build') {
      environment{
        LOG_LEVEL = 'INFO'
      }
      parallel {
        stage('linux-arm64'){
          steps {
            echo "Building release ${RELEASE} for ${STAGE_NAME} with log level ${LOG_LEVEL}...."
          }
        }
        stage('linux-amd64'){
          steps {
            echo "Building release ${RELEASE} for ${STAGE_NAME} with log level ${LOG_LEVEL}...."
          }
        }
       stage('windows-amd64'){
          steps {
            echo "Building release ${RELEASE} for ${STAGE_NAME} with log level ${LOG_LEVEL}...."
          }
        }
      }
    }
    stage('Test'){
      steps{
        echo "Testing. I can see ${RELEASE}..."
        writeFile file: 'test-result.txt',text: 'passed'
      }
    }
    stage('Deploy'){
    input{
      message 'Deploy?'
      ok 'Do it!'
      parameters {
        string(name: 'TARGET_ENVIRONMENT',defaultValue: 'PROD',description: 'Target deployment environment')
      }
    }
    steps{
      echo "Deploying release ${RELEASE} to environment ${TARGET_ENVIRONMENT}"
    }
    }
  }
  post{
    success {
      archiveArtifacts 'test-result.txt'
      slackSend channel: '#q4-budget',
                message: "Release ${env.RELEASE},success: ${currentBuild.fullDisplayName}."
    }
  }
}