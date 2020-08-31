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
         sh "cp -r app resources index.html common.js dist"
        }
      }
    }
  }
}