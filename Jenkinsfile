// The failedStages variable is reused between stages
def failedStages=[]
//Declarative
pipeline {
  agent { label 'RaspberryPi' }
  parameters {
    string(name: 'File_Path', defaultValue: '', description: 'Specify the full path - Optional')
    choice(name: 'File_Type', choices: ['.mp4', '.avi', '.ts', '.mp3'], description: 'Choose the desire file type to convert')
    string(name: 'File_Name', defaultValue: '', description: 'Please specify the filename.')

  }
    
  options {
    timeout(time: 1, unit: 'HOURS')
    //Skip the default checkout to clear the workspace first and then checkout in an "Initialize" stage.
    skipDefaultCheckout()
  }
  
  stages {
    stage('FFMPEG') {
      steps {
      //cleanWs()
      checkout scm
        script {
          //if (params.File_Path != null){
           // if (params.File_Path != null){
              sh "sudo ffmpeg -i ${params.File_Path} /home/pi/Videos/FFMPEG/${params.File_Name}${params.File_Type}"
            //}
          //}
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
    success {
      script {
        telegramSend(message: "Hi Nikos, Your video is ready on '/home/pi/Videos/FFMPEG' path")
        }
      }
    }
}
