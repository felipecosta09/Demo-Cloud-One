pipeline {
  agent any
  stages {
    stage('Git Checkout') {
      steps {
        sh 'echo Git Checkout from the Repository'
        git(url: 'https://github.com/felipecosta09/DSSC.git', branch: 'master', poll: true)
      }
    }

    stage('Source Code Test') {
      steps {
        sleep 10
      }
    }

    stage('Container Build') {
      steps {
        sh '''echo Build the Docker Container
echo 
docker build -t web-app:latest .'''
      }
    }

    stage('Push to ECR') {
      steps {
        sh '''echo Logging in to Amazon ECR...
aws --version
$(aws ecr get-login --no-include-email --region us-east-1)
echo 
echo Tagging Docker Build to prepare to Push
docker tag web-app:latest 650143975734.dkr.ecr.us-east-1.amazonaws.com/web-app
echo 
echo Push the New Image to ECR
docker push 650143975734.dkr.ecr.us-east-1.amazonaws.com/web-app'''
      }
    }

    stage('Cloud One Container Image Scan') {
      steps {
        sh 'echo Container Image Scan from CLoud One'
      }
    }

    stage('Dev Tests') {
      parallel {
        stage('') {
          steps {
            sh 'echo \'TBD\''
          }
        }

        stage('Unit Tests') {
          steps {
            sh 'echo \'TBD\''
          }
        }

        stage('Deploy Tests') {
          steps {
            sh 'echo \'TBD\''
          }
        }

      }
    }

    stage('DS App Protect') {
      steps {
        sh 'echo Deploy New Container to Fargate'
      }
    }

    stage('Deploy') {
      steps {
        echo 'Deploy New Container to Fargate'
      }
    }

    stage('Slack Notification') {
      steps {
        slackSend(channel: '#aws-account-alerts', color: 'good', message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n More info at: ${env.BUILD_URL}")
      }
    }

  }
}