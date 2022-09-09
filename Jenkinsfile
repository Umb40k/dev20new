#!groovy
import groovy.json.JsonSlurperClassic
node {

    def BUILD_NUMBER=env.BUILD_NUMBER
    def RUN_ARTIFACT_DIR="tests/${BUILD_NUMBER}"
    def SFDC_USERNAME

    def HUB_ORG=env.HUB_ORG_DH ?: "upiscopodev@dev.com"
    def SFDC_HOST = env.SFDC_HOST_DH ?: "https://login.salesforce.com"
    def JWT_KEY_CRED_ID = env.JWT_CRED_ID_DH ?:"93cb3a4f-47a9-47f5-b763-ae7cb11d641e"
    def JWT_KEY_FILE= "server.key"
    def DEPLOYDIR='src'
    def TEST_LEVEL='RunLocalTests'



    def CONNECTED_APP_CONSUMER_KEY=env.CONNECTED_APP_CONSUMER_KEY_DH ?: "3MVG9t0sl2P.pByrVcWPZR2GQS5ug2AwIuHx_wOEyisZ_zdfHdP77glvprxNJ15L.Ku7Ivz9ML0aPUVBq2q9Z"

    println 'KEY IS' 
    println JWT_KEY_CRED_ID
    println HUB_ORG
    println SFDC_HOST
    println CONNECTED_APP_CONSUMER_KEY
    println JWT_KEY_FILE
    //printf $JWT_KEY_CRED_ID > server.key
    def toolbelt = tool 'toolbelt'
    //def sfdx = tool 'sfdxtool'
    stage('checkout source') {
        checkout scm
    }

    withEnv(["HOME=${env.WORKSPACE}"]) {

    withCredentials([file(credentialsId: JWT_KEY_CRED_ID, variable: 'jwt_key_file')]) {        
        stage('SFDX Login') {
        //rcl = bat returnStatus: true, script: "\"${toolbelt}\\sfdx\" force:auth:logout --username ${HUB_ORG}"
        rc = bat returnStatus: true, script: "\"${toolbelt}\\sfdx\" force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwtkeyfile \"${jwt_key_file}\" --setdefaultdevhubusername --instanceurl ${SFDC_HOST}"
        if (rc != 0) { error 'hub org authorization failed' 
        }
            
        }
			println rc	
        stage('Check only') {
        ck = bat returnStdout: true, script: "\"${toolbelt}\\sfdx\" force:source:deploy --checkonly --sourcepath force-app -u ${HUB_ORG} --testlevel RunLocalTests "


        stage('Deploy') {
        msg = bat returnStdout: true, script: "\"${toolbelt}\\sfdx\" force:source:deploy --sourcepath force-app -u ${HUB_ORG}"
		        printf msg

        if (msg != 1) {
			error 'Salesforce deploy failed.'
		  }        
        }	
  
        }



     }
    
}