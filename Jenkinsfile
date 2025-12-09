pipeline {
  agent any

  environment {
    AWS_DEFAULT_REGION = 'us-east-1'         
    S3_BUCKET = 'capstone-prj-t2-skbkt'    
  }

  stages {
    stage('Checkout Source') {
      steps {
        echo 'Checking out code from GitHub...'
        git url: 'https://github.com/simonkaggwa-ai/capstone-prj-t2-repo', branch: 'main'
      }
    }

    stage('Validate Files') {
      steps {
        echo 'Checking HTML files...'
        sh 'ls -1 *.html style.css'
      }
    }

    stage('Deploy to S3') {
      steps {
        echo "Deploying site to S3 bucket: ${S3_BUCKET}"
        // Upload files to S3 (overwrite existing, delete removed, make public)
        script {
            withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'jenkins-sk', accessKeyVariable: 'AWS_ACCESS_KEY_ID', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
                sh "aws s3 sync . s3://$S3_BUCKET --delete" 
            }
        }
      }  
    }
    
    stage('Confirm Deployment') {
      steps {
        script {
          def siteURL = "http://${S3_BUCKET}.s3-website-${AWS_DEFAULT_REGION}.amazonaws.com"
          echo "Website deployed successfully!"
          echo "Access it at: ${siteURL}"
        }
      }
    }
  }
}
