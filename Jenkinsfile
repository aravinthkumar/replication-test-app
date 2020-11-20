pipeline {
  agent any
  stages {
    stage('stage1') {
      steps {
        echo 'This is the build number ${env.BUILD_NUMBER} of demo ${env.DEMO}'
        bat(script: 'echo "This is the build number  ${env.BUILD_NUMBER} of demo ${env.DEMO}"', returnStdout: true, returnStatus: true)
      }
    }

  }
  environment {
    DEMO = '1'
  }
}