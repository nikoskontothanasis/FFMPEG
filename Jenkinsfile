// The failedStages variable is reused between stages
def failedStages=[]
def result=[]
//Declarative
pipeline {
  agent { label 'RaspberryPi' }
  //parameters {
      string(name: 'File_Path', defaultValue: '', description: 'Specify the full path - Optional'
      choice(name: 'File_Type', choices: ['mp4', 'avi', 'ts', 'mp3'], description: 'Choose the desire file type to convert')
  }
    
  options {
    timeout(time: 1, unit: 'HOURS')
    //Skip the default checkout to clear the workspace first and then checkout in an "Initialize" stage.
    skipDefaultCheckout()
  }
  
  stages {
    stage('FFMPEG') {
      steps {
      cleanWs()
      checkout scm
        script {
         sh "ffmpeg -i ${File_Path} "
        }
      }
      post {
        failure {
          script { failedStages.add(STAGE_NAME) }
          echo "Failed at stage \"${STAGE_NAME}\" with unhandled exception."
        }
      }      
    }
  }
  post {
    always {
      script {
          if (result<'35'){
            telegramSend(message: "Hi Nikos, Your intenet connection is \"${result}\" Mbps.")
          }
        }
      }
    }
}
