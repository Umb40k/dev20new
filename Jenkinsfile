#!groovy
import groovy.json.JsonSlurperClassic
node {

    def BUILD_NUMBER=env.BUILD_NUMBER
    def RUN_ARTIFACT_DIR="tests/${BUILD_NUMBER}"
    def SFDC_USERNAME

    def HUB_ORG=env.HUB_ORG_DH
    def SFDC_HOST = env.SFDC_HOST_DH
    def JWT_KEY_CRED_ID = env.JWT_CRED_ID_DH
    def JWT_KEY_FILE= "server.key"

    def CONNECTED_APP_CONSUMER_KEY=env.CONNECTED_APP_CONSUMER_KEY_DH

    println 'KEY IS' 
    println JWT_KEY_CRED_ID
    println HUB_ORG
    println SFDC_HOST
    println CONNECTED_APP_CONSUMER_KEY
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

    withCredentials([file(credentialsId: JWT_KEY_CRED_ID, variable: 'JWT_KEY_FILE')]) {        
        stage('Deployment') {
        rc = rc = command '$toolbelt/sfdx auth:jwt:grant --instanceurl $SF_INSTANCE_URL --clientid $SF_CONSUMER_KEY --username $SF_USERNAME --jwtkeyfile $server_key_file --setdefaultdevhubusername --setalias Prod'
        if (rc != 0) { error 'hub org authorization failed' }

			println rc

			  
            printf rmsg
            println(rmsg)
        }
    }
}