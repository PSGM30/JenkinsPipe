pipeline {
  agent any
  stages {
    stage('Clean Reports')
    {
      steps{
        echo '********* Cleaning Workspace Stage Started **********'
        bat 'rmdir /s /q test-reports'
        echo '********* Cleaning Workspace Stage Finished **********'
      }
    }
    
    stage('Build Stage') {
      steps {
        echo '********* Build Stage Started **********'
        
        sh 'python app.py'
        echo '********* Build Stage Finished **********'
        }
    }
    stage('Testing Stage') {
      steps {
        echo '********* Test Stage Started **********'
        sh 'python test.py'
        echo '********* Test Stage Finished **********'
      }   
    }
    stage('Sanity check') {
            steps {
                input "Does the staging environment look ok?"
            }
     }
stage('Deployment Stage'){
            steps{
                input "Do you want to Deploy the application?"
                echo '********* Deploy Stage Started **********'
                timeout(time : 1, unit : 'MINUTES')
                {
                bat 'python app.py'
                }
                echo '********* Deploy Stage Finished **********'
            }
    }
  }
  post {
        always {
            echo 'We came to an end!'
            archiveArtifacts artifacts: 'dist/*.exe', fingerprint: true
            junit 'test-reports/*.xml'
          script{
            if(currentBuild.currentResult=='SUCCESS')
            {
              echo '********* Uploading to Artifactory is Started **********'
              /*bat 'jfrog rt u "dist/*.exe" generic-local'*/
              bat 'Powershell.exe -executionpolicy remotesigned -File build_script.ps1'
              echo '********* Uploading Finished **********'
            }
          }
          
            
            deleteDir()

         }
        success {
          echo 'Build Successfull!!'
    }
        failure {
        echo 'Sorry mate! build is Failed :('
    }
        unstable {
            echo 'Run was marked as unstable'
        }
        changed {
            echo 'Hey look at this, Pipeline state is changed.'
        }
    }
}
