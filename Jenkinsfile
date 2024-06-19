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
     stage('Run Pylint') {
            steps {
                script {
                    def pylintResult = sh(
                        script: '''
                            . ${VENV_DIR}/bin/activate
                            pylint test.py
                        ''',
                        returnStatus: true
                    )

                    if (pylintResult != 0) {
                        error "Pylint checks failed"
                    }
                }
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
		echo 'Pylint checks failed.'
            emailext (
                subject: "Jenkins Job Failed: ${env.JOB_NAME}",
                body: """
                    <p>Build failed in Jenkins job ${env.JOB_NAME}.</p>
                    <p>Check console output at <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                """,
                recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']],
                to: "${env.EMAIL_RECIPIENTS}"
            )

}

}

}
