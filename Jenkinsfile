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
        script {
          // Build the Docker image with a specific name
          sh 'docker build -t your-app-image .'
        }
      }
    }
    stage('Push Docker Image to ECR') {
      steps {
        script {
          // Tag the Docker image with the ECR repository URL
          sh 'docker tag your-app-image:latest 323526869397.dkr.ecr.us-west-2.amazonaws.com/myrepo2:latest'
          // Authenticate Docker to Amazon ECR
          sh 'aws ecr get-login-password --region us-west-2 | docker login --username AWS --password-stdin 323526869397.dkr.ecr.us-west-2.amazonaws.com'
          // Push the Docker image to Amazon ECR
          sh 'docker push 323526869397.dkr.ecr.us-west-2.amazonaws.com/myrepo2:latest'
        }
      }
    }
    stage('Deploy to ECS') {
    steps {
        withCredentials([aws(accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'useraleem-aws-creds', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
            // Update ECS service with the new Docker image
            sh 'aws ecs update-service --cluster jenkinscluster --service jenkinsservice --force-new-deployment --region us-west-2'
        }
      }
    }
  }
}
