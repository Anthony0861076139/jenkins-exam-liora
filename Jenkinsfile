pipeline {
environment {
DOCKER_ID = "anthony086"
DOCKER_PASS = credentials("DOCKER_HUB_PASS")
DOCKER_MOVIE_IMAGE = "movie-app-image"
DOCKER_CAST_IMAGE = "cast-app-image"
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
      cd ${WORKSPACE}/app/cast-service
      docker build . -t ${DOCKER_CAST_IMAGE}:latest
      docker tag ${DOCKER_CAST_IMAGE} ${DOCKER_ID}/${DOCKER_CAST_IMAGE}
      docker image push ${DOCKER_ID}/${DOCKER_CAST_IMAGE}:latest
      '''
      }
    }
  stage ("deploy dev env") {
    steps {
      sh '''
      rm -Rf ~/.kube/
      mkdir ${WORKSPACE}/.kube
      cat ${KUBECONFIG} > ${WORKSPACE}/.kube/config
      helm upgrade install jenkins-exam-liora ./helm-chart --values=./helm-chart/values-dev.yaml
      '''
      }
    }
  }
}