pipeline {
  agent any
  parameters{
    booleanParam(name: 'RC',defaultValue: false,description: 'Is a Release Candidate?')
  }
    environment {
    RELEASE = '20.11'
    VERSION = "0.1.0"
    VERSION_RC = "rc.2"
  }
  stages {
    stage('Audit Tools'){
      steps {
           auditTools()
      }
    }
    stage('Build') {
      environment{
        LOG_LEVEL = 'INFO'
        VERSION_SUFFIX = getVersionSuffix()
      }
      parallel {
        stage('linux-arm64'){
          steps {
            echo "Building release ${VERSION} with suffix: ${VERSION_SUFFIX} for ${STAGE_NAME} with log level ${LOG_LEVEL}...."
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
        script {
          if (Math.random() > 0.5) {
            throw new Exception()
          }
        }
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
    stage('Publish'){
      when {
        expression { return params.RC }
      }
      steps {
        bat ''' echo Published '''
      }
    }
  }
  post{
    success {
      archiveArtifacts 'test-result.txt'
      slackSend channel: '#q4-budget',
                color: 'good',
                message: "Release ${env.RELEASE}, success: ${currentBuild.fullDisplayName}."
    }
    failure {
      slackSend channel: '#q4-budget',
                color: 'danger',
                message: "Release ${env.RELEASE}, FAILED: ${currentBuild.fullDisplayName}."
    }
  }
}

void auditTools(){
  bat '''
  git version
  cf --version
  '''

}

String getVersionSuffix(){
  if (params.RC){
    return env.VERSION_RC
    else{
      return env.VERSION_RC + 'ci' + env.BUILD_NUMBER
    }
  }
}