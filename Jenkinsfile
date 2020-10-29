// Powered by Infostretch 

timestamps {

node () {

	stage ('Android_build - Checkout') {
 	 checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'github', url: 'https://github.com/jmzsec/pivaa.git']]]) 
	}
	stage ('Android_build - Build') {
 	
// Unable to convert a build step referring to "hudson.plugins.ws__cleanup.PreBuildCleanup". Please verify and convert manually if required.		// Shell build step
sh """ 
cd /home/jm/projetos && gradle clean assemble --no-daemon -x lintVitalRelease 
 """		// Shell build step
sh """ 
curl -F 'file=@/home/jm/projetos/apks/pivaa.apk' http://localhost:8000/api/v1/upload -H "X-Mobsf-Api-Key:8176dec5ce2b50f71eb7759b43439b69eac8bdca572b686d7741abf0c14ff239" | awk -F'[/"]' '{print $8}' > hash.txt 
 """		// Shell build step
sh """ 
curl -X POST --url http://localhost:8000/api/v1/scan --data "scan_type=apk&file_name=pivaa.apk&hash=$(cat hash.txt)" -H "X-Mobsf-Api-Key:8176dec5ce2b50f71eb7759b43439b69eac8bdca572b686d7741abf0c14ff239" 
 """		// Shell build step
sh """ 
sleep 120 
 """		// Shell build step
sh """ 
curl -X POST --url http://localhost:8000/api/v1/download_pdf --data "hash=$(cat hash.txt)" -H "X-Mobsf-Api-Key:8176dec5ce2b50f71eb7759b43439b69eac8bdca572b686d7741abf0c14ff239" -o MobSF${BUILD_ID}.pdf 
 """ 
	}
}
}