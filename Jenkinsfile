pipeline {
    agent any
    enviornment{

        GIT_CREDS_ID = 'github_cred'
        GIT_REPO_URL = 'https://github.com/sbavania/jenkinsTest.git'
    }
    stages{
      stage('Build'){
          steps{

            sh 'echo printing.. $GIT_REPO_URL'
          }

      }

    }


}
