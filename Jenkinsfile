#!groovy
import groovy.json.JsonSlurperClassic
node {

    def BUILD_NUMBER=env.BUILD_NUMBER
    def RUN_ARTIFACT_DIR="tests/${BUILD_NUMBER}"
    def SFDC_USERNAME

    def HUB_ORG=env.HUB_ORG_DH
    def SFDC_HOST = env.SFDC_HOST_DH
    def JWT_KEY_CRED_ID = env.JWT_CRED_ID_DH
    def CONNECTED_APP_CONSUMER_KEY=env.CONNECTED_APP_CONSUMER_KEY_DH
    def PACKAGE_KEY = env.SF_PACKAGE_KEY
    def PACKAGE_ID = env.SF_PACKAGE_ID
	
    println 'CONFIGURATIONS' 
    println JWT_KEY_CRED_ID
    println HUB_ORG
    println SFDC_HOST
    println CONNECTED_APP_CONSUMER_KEY
    println 'Start the job'

    withCredentials([file(credentialsId: JWT_KEY_CRED_ID, variable: 'jwt_key_file')]) {
        stage('Deploye Code') {
          
                 rc = bat returnStatus: true, script: "sfdx force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwtkeyfile \"${jwt_key_file}\" --setdefaultdevhubusername --instanceurl ${SFDC_HOST}"
            
            if (rc != 0) { error 'Org authorization failed' }

			println rc
			rmsg = bat returnStdout: true, script: "sfdx force:package:install --package ${PACKAGE_ID}  --installationkey \" ${PACKAGE_KEY}\" -u ${HUB_ORG} --wait -1"
			
			  
            printf rmsg
            println(rmsg)
        }
    }
}
