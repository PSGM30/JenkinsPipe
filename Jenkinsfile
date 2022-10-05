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
    
    stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t basangouda/dockertrial .'
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                   withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                   sh 'docker login -u basangouda -p BPDataScientist@24'

}
                   sh 'docker push basangouda/dockertrial'
                }
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
                sh 'python app.py'
                }
                echo '********* Deploy Stage Finished **********'
            }
    }
  }
  post {
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
