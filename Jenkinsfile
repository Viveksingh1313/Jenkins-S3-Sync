region = 'us-west-2'
bucket_name = 'jenkins-s3-sync'
gitBranch = 'develop'

pipeline {
  agent any
  stages {
    stage('info') {
      steps {
        milestone 1
        // Jenkins doesn't expose the hash of the commit being built, so we'll
         sh "mkdir dist"
         sh "cp -r index.html dist"
                 withCredentials([ [$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'AKIA2THY23XZE2DR2OUR (Jenkins AWS connect)']])
                 {
                   echo "After withCredentials"
                   dir('dist') {
                     echo "Inside dist - initiating aws s3 sync"
                     sh "aws s3 sync --region ${region} . arn:aws:s3:::jenkins-s3-sync --cache-control no-cache,no-store,must-revalidate,max-age=3600 --delete"
                    }
                 }
        }
      }
  }
}