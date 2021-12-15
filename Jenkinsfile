pipeline {
    agent any
    stages {
	    stage('Clone'){
	        steps{
			    bat("""git clone https://github.com/klltx/spring-petclinic""")
	        }
	    }
        stage('Build'){
            steps{
                bat('C:\\Users\\Administrator\\Desktop\\spring-petclinic\\build.bat')
            }
	    }
        stage('Archive'){
            steps{
                dir('C:\\'){
                    zip zipFile: "artifacts\\${BUILD_NUMBER}.zip", archive:false, dir: 'Windows\\System32\\config\\systemprofile\\AppData\\Local\\Jenkins\\.jenkins\\workspace\\petclinicJob'
                    archiveArtifacts artifacts: "artifacts\\${BUILD_NUMBER}.zip"
                }
            }
        }
        stage('Deploy'){
            steps{
                dir('C:\\'){
                    script{
                        try
                        {
                            bat("md deploy")
                        }catch(Exception e){}
                    }
                    unzip zipFile: "artifacts\\${BUILD_NUMBER}.zip", dir: 'deploy'
                }
            }
	    }
    }

    post {
	  always{
		emailext attachLog: true, body: '''$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS:
		Check console output at $BUILD_URL to view the results.''', subject: '$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!', to: 'kostya.svetashov.01@gmail.com'
	  }
          cleanup {
              cleanWs()
          }
   }
}
