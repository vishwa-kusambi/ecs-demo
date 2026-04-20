pipeline {
  agent any

  environment {
    ECR_URI = '205091463760.dkr.ecr.ap-south-1.amazonaws.com/my-app'
    AWS_REGION = 'ap-south-1'
  }

  stages {

    stage('Checkout') {
      steps {
        git 'https://github.com/vishwa-kusambi/ecs-demo.git'
      }
    }

    stage('Build') {
      steps {
        sh 'docker build -t my-app .'
      }
    }

    stage('Login to ECR') {
      steps {
        sh '''
        aws ecr get-login-password --region $AWS_REGION \
        | docker login --username AWS --password-stdin 205091463760.dkr.ecr.ap-south-1.amazonaws.com
        '''
      }
    }

    stage('Push Image') {
      steps {
        sh '''
        docker tag my-app:latest $ECR_URI:latest
        docker push $ECR_URI:latest
        '''
      }
    }

    stage('Deploy ECS') {
      steps {
        sh '''
        aws ecs update-service \
        --cluster ecs-demo-cluster \
        --service ecs-demo-task-service-d0zeye5f \
        --force-new-deployment
        '''
      }
    }
  }
}