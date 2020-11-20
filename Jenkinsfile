pipeline {
  agent any
  stages {
    stage('stage1') {
      steps {
        echo 'This is the build number ${env.BUILD_NUMBER} of demo ${env.DEMO}'
        bat 'echo "This is the build number  ${env.BUILD_NUMBER} of demo ${env.DEMO}"'
        sh 'echo " Build ${env.BUILD_NUMBER}"'
      }
    }

  }
  environment {
    DEMO = '1'
  }
}