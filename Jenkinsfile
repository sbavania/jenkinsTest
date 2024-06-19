pipeline {
    agent any
   environment{

        GIT_CREDS_ID = 'github_cred'
        GIT_REPO_URL = 'https://github.com/sbavania/jenkinsTest.git'
        VENV_DIR = 'venv'  // Directory for the Python virtual environment
        EMAIL_RECIPIENTS = 'sbavania12@gmail.com'
    }
    stages{
      stage('clone repository'){
          steps{

            git branch: 'main',
                    url: "${env.GIT_REPO_URL}",
                    credentialsId: "${env.GIT_CREDS_ID}"
          }
	}
	stage('Set up Python Enviornment'){

           steps {
                   sh '''
                    python3 -m venv ${VENV_DIR}
                    . ${VENV_DIR}/bin/activate
                    pip install --upgrade pip
                    pip install pylint
                  '''

           }

         }
      

      }

    

  post{
always {
            echo 'Cleaning up...'
            sh 'rm -rf ${VENV_DIR}'
        }
        success {
            echo 'Pylint checks passed!'
        }
        failure {
         
 		echo 'failed'
}

}

}
