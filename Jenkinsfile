pipeline {
  agent any
  stages {
    stage('Build Docker image') {
      steps {
        sh '''__ver=$(cat VERSION)
docker build -t demo-image-develop:${__ver} .'''
      }
    }
    stage('Docker login') {
          steps {
            sh 'docker login -u admin -p admin mycluster.icp:8500'
          }
        }
        stage('Docker tag & push to ICP registry') {
          when {
              branch 'master'
          }
          steps {
            sh '''__ver=$(cat VERSION)
docker tag demo-image-develop:${__ver} mycluster.icp:8500/default/demo-image:${__ver}
docker push mycluster.icp:8500/default/demo-image:${__ver}
'''
          }
        }
    stage('bx pr login') {
      when {
              branch 'master'
      }
      steps {
        sh ' bx pr login -a https://mycluster.icp:8443 --skip-ssl-validation -u admin -p admin -c id-mycluster-account'
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
}