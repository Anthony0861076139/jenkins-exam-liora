pipeline {
environment {
DOCKER_ID = "anthony086"
DOCKER_PASS = credentials("DOCKER_HUB_PASS")
DOCKER_MOVIE_IMAGE = "movieAppImage"
}

agent any 

stages {
  stage ("Docker build & push stage") {
    steps {
      sh '''
      cd ${WORKSPACE}/app/movie-service
      docker build . -t ${DOCKER_MOVIE_IMAGE}:latest
      docker image ls
      docker tag ${DOCKER_MOVIE_IMAGE} ${DOCKER_ID}/${DOCKER_MOVIE_IMAGE}
      docker login -u anthony086 -p ${DOCKER_PASS}
      docker image push ${DOCKER_ID}/${DOCKER_MOVIE_IMAGE}:latest
      '''
      }
    }
  }
}