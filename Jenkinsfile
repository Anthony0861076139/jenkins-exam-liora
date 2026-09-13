pipeline {
environment {
DOCKER_ID = anthony086
DOCKER_PASS = credentials ("DOCKER_HUB_PASS")
}

  stages {
    stage ("Docker build stage")
      steps {
        sh '''
        echo 'this is a test'
        '''
      }
  }
}