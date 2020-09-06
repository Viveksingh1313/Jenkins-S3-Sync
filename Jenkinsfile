region = 'us-west-2'
bucket_name = 'jenkins-s3-sync'
gitBranch = 'develop'

pipeline {
  agent {
    docker {
            image 'alpine:3.12.0'
            args '-u root'
        }
  }
  environment {
    AWS_ID = credentials("jenkins-s3")
    AWS_ACCESS_KEY_ID = "${env.AWS_ACCESS_KEY_ID}"
    AWS_SECRET_ACCESS_KEY = "${env.AWS_SECRET_ACCESS_KEY}"
    }
  stages {
    stage('info') {
      steps {
        milestone 1
        // Jenkins doesn't expose the hash of the commit being built, so we'll
         sh "mkdir dist"
         sh "cp -r index.html dist"
         echo AWS_ACCESS_KEY_ID
                 withCredentials([ [$class: 'AmazonWebServicesCredentialsBinding',
                 credentialsId: '$(AWS_ID)',
                 accessKeyVariable:'$(AWS_ACCESS_KEY_ID)',
                 secretKeyVariable:'$(AWS_SECRET_ACCESS_KEY)'
                 ]]){
                    echo "After withCredentials"
                    echo AWS_SECRET_ACCESS_KEY
                    dir('dist') {
                    echo "Inside dist - initiating aws s3 sync"
                    sh "aws s3 sync --region ${region} . s3://jenkins-s3-sync"
                    }
                 }
        }
      }
  }
  post {
      always {
        // Clean up workspace, so we don't fill up the build box
        cleanWs notFailBuild: true
      }
  }
}