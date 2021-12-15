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
                    script{
                        try
                        {
                            bat("md artifacts")
                        }catch(Exception e){}
                    }
                    zip zipFile: "artifacts\\${BUILD_NUMBER}.zip", archive:false, dir: 'target'
                    archiveArtifacts artifacts: "artifacts\\${BUILD_NUMBER}.zip"
                }
            }
        }
        stage('Deploy'){
            steps{
                script{
                    try
                    {
                        bat("md C:\\deploy\\")
                    }catch(Exception e){}
                }
                unzip zipFile: "${BUILD_NUMBER}.zip", dir: 'C:\\deploy'
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
