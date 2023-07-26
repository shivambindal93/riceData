pipeline {
  agent any
  environment {
    dockerUsr='nagpshivam'
    dockerPwd='dckr_pat_SHIZiJ3k32kWGJVyTN87s8Basmc'
    KUBECONFIG='kube/config'
  }
  tools {
    nodejs 'nodejs'
  }
  stages {
    stage('Install') {
      steps {
        sh 'npm install'
      }
    }
    stage('Build') {
      steps {
        sh 'docker build -t nagpshivam/ricedata:latest .'
        sh 'docker login -u ${dockerUsr} -p ${dockerPwd}'
        sh 'docker push nagpshivam/ricedata:latest'
      }
    }
    stage('Kubernetes Deployment') {
      steps {
        // sh 'gcloud auth activate-service-account --key-file="$GCLOUD_CREDS"'
        // sh "gcloud container clusters get-credentials ${clusterName} --zone ${zone} --project ${gcloudProject}"
        // sh 'gcloud auth login --kubeconfig=kube/config'
        sh 'kubectl --kubeconfig=${KUBECONFIG} delete -f k8s/deployment.yaml'
        // sh 'kubectl apply -f k8s/configMap.yaml --force --grace-period=0'
        sh 'kubectl --kubeconfig=${KUBECONFIG} apply -f k8s/deployment.yaml --force --overwrite --grace-period=0'
        sh 'kubectl --kubeconfig=${KUBECONFIG} apply -f k8s/service.yaml --force --grace-period=0'
       // sh 'kubectl apply -f k8s/ingress.yaml --force --grace-period=0'
      }
    }
  }
}
