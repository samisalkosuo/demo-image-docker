pipeline {
  agent any
  stages {
    stage('Build Docker image') {
      steps {
        sh 'docker build -t demo-image-develop .'
      }
    }
    stage('Configure Kubectl') {
      parallel {
        stage('Configure Kubectl') {
          steps {
            sh '''kubectl config set-cluster mycluster.icp --server=https://169.50.29.248:8001 --insecure-skip-tls-verify=true
kubectl config set-context mycluster.icp-context --cluster=mycluster.icp
kubectl config set-credentials admin --token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJhdF9oYXNoIjoienM3N2NibjF3dW5uZnR1M2puMXIiLCJyZWFsbU5hbWUiOiJjdXN0b21SZWFsbSIsInVuaXF1ZVNlY3VyaXR5TmFtZSI6ImFkbWluIiwiaXNzIjoiaHR0cHM6Ly9teWNsdXN0ZXIuaWNwOjk0NDMvb2lkYy9lbmRwb2ludC9PUCIsImF1ZCI6IjA4MjVjNDc1MTVlYjUwOWY3ZWI0M2NiOTBkN2E1ZGQyIiwiZXhwIjoxNTI3NjYxMDQ2LCJpYXQiOjE1Mjc2NjEwNDYsInN1YiI6ImFkbWluIiwidGVhbVJvbGVNYXBwaW5ncyI6W119.o7olOjmTGc83RehGc8PmTYbqm8Gv5I7wcDRSZXneeaSU1ZZihhFXyRZb6BziU5piKGlrb9rMkcNB9O0nCuotY4T3iIprtAV_aJ8egeDoOKuj1jRF4NXazxYOTP7vCcejVEJ4cMG8I_dS3YAtpRkqf3QFIvLlmZGCt43xH0MRmVocy66SPwUvwsRR71er90-dXZAkK0r2QtFtioLHjRqoU_a_JZHJ0p_1_AMoQc9dHALdIAqgehapRv2eRGqADv31qthF7EiHJzgUoNs_8Zg2ZVCri0U5Z3e_qUH-y7Mt-YB_TULJozf0rIk7WX8lCwMcd0jRpES_GLkCBMelPikPLw
kubectl config set-context mycluster.icp-context --user=admin --namespace=default
kubectl config use-context mycluster.icp-context
'''
          }
        }
        stage('Docker login') {
          steps {
            sh 'docker login -u admin -p admin mycluster.icp:8500'
          }
        }
      }
    }
    stage('Kubectl info') {
      parallel {
        stage('Kubectl info') {
          steps {
            sh 'kubectl cluster-info'
          }
        }
        stage('Docker tag & push') {
          steps {
            sh '''docker tag demo-image-develop:latest mycluster.icp:8500/default/demo-image:develop
docker push mycluster.icp:8500/default/demo-image:develop
'''
          }
        }
      }
    }
    stage('bx pr login') {
      steps {
        sh ' bx pr login -a https://mycluster.icp:8443 --skip-ssl-validation -u admin -p admin -c id-mycluster-account'
      }
    }
  }
}