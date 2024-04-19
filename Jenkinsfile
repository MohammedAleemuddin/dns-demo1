pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', credentialsId: 'your-aws-creds', url: 'https://github.com/devam-yorkie/cdk-copilot-deploy.git'
      }
    }
    stage('Build Docker Image') {
      steps {
        sh 'docker build -t your-app-image .'
      }
    }
    stage('Push Docker Image to ECR') {
      steps {
        withCredentials([aws(accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'useraleem-aws-creds', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
          sh 'aws ecr get-login-password --region us-west-2 | docker login --username AWS --password-stdin 323526869397.dkr.ecr.us-west-2.amazonaws.com'
          sh 'docker tag myrepo2:latest 323526869397.dkr.ecr.us-west-2.amazonaws.com/myrepo2:latest'
        }
      }
    }
    stage('Deploy to ECS') {
      steps {
        withCredentials([aws(accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'useraleem-aws-creds', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
          sh 'aws ecs update-service --cluster your-cluster-name --service your-service-name --force-new-deployment'
        }
      }
    }
  }
}
