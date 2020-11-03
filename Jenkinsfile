
pipeline {
    agent any 

    stages {

 		stage('SAST') {
			agent {
                docker { image 'docker:dind' }
	            }
				
            steps {
                echo "Horusec"
	            
				sh 'curl -fsSL https://horusec-cli.s3.amazonaws.com/install.sh | bash'
                sh 'horusec start -p="./" -e="true"'

                //sh 'horusec start -p="./"'
            }
        }


		stage ('Build') {
            steps { 
			    sh """ 
				    cd /home/jm/projetos && gradle clean assemble --no-daemon -x lintVitalRelease 
 			    """
            }
        }

		stage ('MobSF') {
			steps {
				sh '''curl -F \'file=@/home/jm/projetos/apks/pivaa.apk\' http://localhost:8000/api/v1/upload -H "X-Mobsf-Api-Key:8176dec5ce2b50f71eb7759b43439b69eac8bdca572b686d7741abf0c14ff239" | awk -F\'[/"]\' \'{print $8}\' > hash.txt'''
				sh 'curl -X POST --url http://localhost:8000/api/v1/scan --data "scan_type=apk&file_name=pivaa.apk&hash=$(cat hash.txt)" -H "X-Mobsf-Api-Key:8176dec5ce2b50f71eb7759b43439b69eac8bdca572b686d7741abf0c14ff239"'
				sh 'curl -X POST --url http://localhost:8000/api/v1/download_pdf --data "hash=$(cat hash.txt)" -H "X-Mobsf-Api-Key:8176dec5ce2b50f71eb7759b43439b69eac8bdca572b686d7741abf0c14ff239" -o MobSF${BUILD_ID}.pdf'
			}
		}
	}

}