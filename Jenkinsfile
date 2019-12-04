pipeline {
    environment {
        PROJECT_ID = ''
        CLUSTER_NAME = ''
        LOCATION = ''
        CREDENTIALS_ID = ''
    }
  agent {
    kubernetes {
      label 'sample-app'
      defaultContainer 'jnlp'
      yaml """
apiVersion: v1
kind: Pod
metadata:
labels:
  component: ci
spec:
  # Use service account that can deploy to all namespaces
  serviceAccountName: cd-jenkins
  containers:
  - name: dotnet
    image: dotnet
    command:
    - cat
    tty: true
  - name: gcloud
    image: gcr.io/cloud-builders/gcloud
    command:
    - cat
    tty: true
  - name: kubectl
    image: gcr.io/cloud-builders/kubectl
    command:
    - cat
    tty: true
"""
}
  }
    stages {
        stage('Build'){
            steps{
                container('dotnet'){
                    sh "dotnet build --configuration Release"
                }
            }
        }
            
        /*stage('Deploy to GKE') {
            steps{
                writeFile file: 'manifest.yaml', text: '''apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:1.7.9
          ports:
            - containerPort: 80'''
                container ('kubectl'){
                    step([
                    $class: 'KubernetesEngineBuilder',
                    namespace:'default',
                    projectId: env.PROJECT_ID,
                    clusterName: env.CLUSTER_NAME,
                    location: env.LOCATION,
                    manifestPattern: 'manifest.yaml',
                    credentialsId: env.CREDENTIALS_ID,
                    verifyDeployments: true])
                }
            }   
        }*/
    }
}
