pipeline {
  agent any
  stages {
    stage('stage1') {
      steps {
        echo 'This is the build number ${env.BUILD_NUMBER} of demo ${env.DEMO}'
        bat 'echo "This is the build number  ${env.BUILD_NUMBER} of demo ${env.DEMO}"'
      }
    }

  }
  environment {
    DEMO = '1'
  }
}