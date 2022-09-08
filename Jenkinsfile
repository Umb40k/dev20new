#!groovy
import groovy.json.JsonSlurperClassic
node {

    def BUILD_NUMBER=env.BUILD_NUMBER
    def RUN_ARTIFACT_DIR="tests/${BUILD_NUMBER}"
    def SFDC_USERNAME

    def HUB_ORG=env.HUB_ORG_DH ?: "upiscopodev@dev.com"
    def SFDC_HOST = env.SFDC_HOST_DH ?: "https://login.salesforce.com"
    def JWT_KEY_CRED_ID = env.JWT_CRED_ID_DH ?:"93cb3a4f-47a9-47f5-b763-ae7cb11d641e"
    def JWT_KEY_FILE= "C:/Program Files/sfdx/bin/server.key"


    def CONNECTED_APP_CONSUMER_KEY=env.CONNECTED_APP_CONSUMER_KEY_DH ?: "3MVG9t0sl2P.pByrVcWPZR2GQS5ug2AwIuHx_wOEyisZ_zdfHdP77glvprxNJ15L.Ku7Ivz9ML0aPUVBq2q9Z"

    println 'KEY IS' 
    println JWT_KEY_CRED_ID
    println HUB_ORG
    println SFDC_HOST
    println CONNECTED_APP_CONSUMER_KEY
    println JWT_KEY_FILE
    //printf $JWT_KEY_CRED_ID > server.key
    def toolbelt = tool 'toolbelt'
    def sfdx = tool 'sfdxtool'



        stage('Check branch name') { // Primer paso para compobar que estamos en la rama correcta
        println env.BRANCH_NAME
        if(env.BRANCH_NAME=="master"){// Este nombre es el que le demos al seleccionar el repositorio dentro del Pipeline
            println('Script from master!')
            println sfdx
        }
        else{
            println sfdx
            error 'Incorrect branch' 
        }
        checkout scm
    }

    withEnv(["HOME=${env.WORKSPACE}"]) {

    withCredentials([file(credentialsId: JWT_KEY_CRED_ID, variable: 'server_key_file')]) {        
        stage('Deployment') {
                rc = command "${toolbelt}/sfdx force:auth:jwt:grant --instanceurl ${SFDC_HOST} --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwtkeyfile ${server_key_file} --setdefaultdevhubusername --setalias HubOrg"
        if (rc != 0) { error 'hub org authorization failed' }

			println rc

			  
            printf rmsg
            println(rmsg)
        }
     }
    }
}