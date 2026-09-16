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
  stage ("deploy in dev") {
    environment {
      KUBECONFIG = credentials("config")
    }
    steps {
      sh '''
      rm -Rf ${WORKSPACE}/.kube/
      mkdir ${WORKSPACE}/.kube
      cat ${KUBECONFIG} > ${WORKSPACE}/.kube/config
      echo $DOCKER_TAG
      cp helm-chart/values-dev.yaml values-dev.yaml
      sed -i "s+tag.*+tag : ${DOCKER_TAG}+g" values-dev.yaml
      kubectl config current-context
      helm upgrade --install jenkins-exam-liora ./helm-chart --values=values-dev.yaml -n dev
      '''
      }
    }
    stage ("deploy in qa") {
    environment {
      KUBECONFIG = credentials("config")
    }
    steps {
      sh '''
      rm -Rf ${WORKSPACE}/.kube/
      mkdir ${WORKSPACE}/.kube
      cat ${KUBECONFIG} > ${WORKSPACE}/.kube/config
      echo $DOCKER_TAG
      cp helm-chart/values-qa.yaml values-qa.yaml
      sed -i "s+tag.*+tag : ${DOCKER_TAG}+g" values-qa.yaml
      kubectl config current-context
      helm upgrade --install jenkins-exam-liora ./helm-chart --values=values-qa.yaml -n qa
      '''
      }
    }
    stage ("deploy in staging") {
    environment {
      KUBECONFIG = credentials("config")
    }
    steps {
      sh '''
      rm -Rf ${WORKSPACE}/.kube/
      mkdir ${WORKSPACE}/.kube
      cat ${KUBECONFIG} > ${WORKSPACE}/.kube/config
      echo $DOCKER_TAG
      cp helm-chart/values-staging.yaml values-staging.yaml
      sed -i "s+tag.*+tag : ${DOCKER_TAG}+g" values-staging.yaml
      kubectl config current-context
      helm upgrade --install jenkins-exam-liora ./helm-chart --values=values-staging.yaml -n staging
      '''
      }
    }
    stage ("deploy in prod") {
    environment {
      KUBECONFIG = credentials("config")
    }
    steps {
      sh '''
      rm -Rf ${WORKSPACE}/.kube/
      mkdir ${WORKSPACE}/.kube
      cat ${KUBECONFIG} > ${WORKSPACE}/.kube/config
      echo $DOCKER_TAG
      cp helm-chart/values-prod.yaml values-prod.yaml
      sed -i "s+tag.*+tag : ${DOCKER_TAG}+g" values-prod.yaml
      kubectl config current-context
      helm upgrade --install jenkins-exam-liora ./helm-chart --values=values-prod.yaml -n prod
      '''
      }
    }
  }
}