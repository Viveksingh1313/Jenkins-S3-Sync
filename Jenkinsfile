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
  stages {
    stage('info') {
      steps {
        milestone 1
        // Jenkins doesn't expose the hash of the commit being built, so we'll
         sh "mkdir dist"
         sh "cp -r index.html dist"
                 withCredentials([ [$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'jenkins-s3']])
                 {
                   echo "After withCredentials"
                   dir('dist') {
                        curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
                        unzip awscliv2.zip
                        sudo ./aws/install
                     echo "Inside dist - initiating aws s3 sync"
                     sh "aws s3 sync --region ${region} . s3://jenkins-s3-sync --cache-control no-cache,no-store,must-revalidate,max-age=3600 --delete"
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