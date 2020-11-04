
pipeline {
    agent any 
	environment {

		TESTE="file=@/home/jm/devops/pivaa/app/build/outputs/apk/debug/app-debug.apk"
		WORK_DIR="/home/jm/devops/pivaa/app/build/outputs/apk/debug/"
		WORK_FILE="app-debug.apk"
		MOBSF_APIKEY="8176dec5ce2b50f71eb7759b43439b69eac8bdca572b686d7741abf0c14ff239"
	}
    stages {

 		stage('SAST') {
            steps {
	            echo "horus"
				//sh 'curl -fsSL https://horusec-cli.s3.amazonaws.com/install.sh | bash'
                //sh 'horusec start -p="./"'

            }
        }

		stage ('Build') {
            steps { 
			    sh """ 
					cd /home/jm/devops/pivaa && gradle clean assembleDebug --no-daemon -x lintVitalRelease
	 			   """
            }
        }

		stage ('MobSF') {
			steps {
				sh '''curl -F \ 'file=@/home/jm/devops/pivaa/app/build/outputs/apk/debug/app-debug.apk\' http://localhost:8000/api/v1/upload -H "X-Mobsf-Api-Key:8176dec5ce2b50f71eb7759b43439b69eac8bdca572b686d7741abf0c14ff239" | awk -F\'[/"]\' \'{print $8}\' > hash.txt'''
				sh 'curl -X POST --url http://localhost:8000/api/v1/scan --data "scan_type=apk&file_name=app-debug.apk&hash=$(cat hash.txt)" -H "X-Mobsf-Api-Key:8176dec5ce2b50f71eb7759b43439b69eac8bdca572b686d7741abf0c14ff239"'
				sh 'curl -X POST --url http://localhost:8000/api/v1/download_pdf --data "hash=$(cat hash.txt)" -H "X-Mobsf-Api-Key:8176dec5ce2b50f71eb7759b43439b69eac8bdca572b686d7741abf0c14ff239" -o MobSF${BUILD_ID}.pdf'
			}
		}
	}

}