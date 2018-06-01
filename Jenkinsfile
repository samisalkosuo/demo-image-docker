pipeline {
  agent any
  stages {
    stage('Build Docker image PROD') {
      when {
        branch 'master'
      }
      steps {
        sh '''__ver=$(cat VERSION)
docker build -t ${DOCKER_IMAGE_PROD}:${__ver} .'''
      }
    }
    stage('Build Docker image DEV') {
      when {
        branch 'develop'
      }
      steps {
        sh '''__ver=$(cat VERSION)
docker build -t ${DOCKER_IMAGE_DEV}:${__ver} .
ls -latr'''
      }
    }
    stage('Docker login') {
      steps {
        sh 'docker login -u ${ICP_USERNAME} -p ${ICP_PASSWORD} ${ICP_REGISTRY_IP}'
      }
    }
    stage('Docker tag & push to ICP registry') {
      steps {
        sh '''__ver=$(cat VERSION)
docker tag demo-image-develop:${__ver} ${ICP_REGISTRY_IP}/default/demo-image:${__ver}
docker push ${ICP_REGISTRY_IP}/default/demo-image:${__ver}
'''
      }
    }
    stage('bx pr login') {
      when {
        branch 'master'
      }
      steps {
        sh ' bx pr login -a https://mycluster.icp:8443 --skip-ssl-validation -u ${ICP_USERNAME} -p ${ICP_PASSWORD} -c id-mycluster-account'
      }
    }
    stage('deploy development') {
      when {
        branch 'develop'
      }
      steps {
        sh ' echo Hello for DEVELOPMENT'
      }
    }
    stage('deploy production') {
      when {
        branch 'master'
      }
      steps {
        sh ' echo Hello for PROD'
      }
    }
  }
  environment {
    DOCKER_IMAGE_PROD = 'demo-image-master'
    DOCKER_IMAGE_DEV = 'demo-image-develop'
    ICP_USERNAME = 'admin'
    ICP_PASSWORD = 'ICPdemo007'
    ICP_REGISTRY_IP = 'mycluster.icp:8500'
  }
}