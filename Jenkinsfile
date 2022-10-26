pipeline {



  agent any



  tools{



  gradle 'gradle1'



  }



  stages {



      stage('gitclone'){



          steps{
                catchError(buildResult: 'success', stageResult: 'FAILURE') {
                sh '''
                   git clone https://github.com/balavu/appiumJAVA.git
                   ls -la
                   '''
                }
            }



      }



      stage('Build') {
           steps {
               sh '''
                   gradle init
                   gradle build
                   ls -a
                  '''
            }
       }
       stage('Push to artifactory') {
           steps {
               sh '''
                   gradle build artifactoryPublish
                   ls -a
                  '''
            }
       }
   }
   
}
