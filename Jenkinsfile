#!groovy
import groovy.json.JsonSlurperClassic
node {

    def BUILD_NUMBER=env.BUILD_NUMBER
    def RUN_ARTIFACT_DIR="tests/${BUILD_NUMBER}"
    def SFDC_USERNAME

    def HUB_ORG="upiscopodev@dev.com"
    def SFDC_HOST = "https://login.salesforce.com"
    def JWT_KEY_CRED_ID = "6232b727-0f64-4796-9d3a-87fb7263d812"
    def JWT_KEY_FILE= "server.key"

    def CONNECTED_APP_CONSUMER_KEY="3MVG9t0sl2P.pByrVcWPZR2GQS5ug2AwIuHx_wOEyisZ_zdfHdP77glvprxNJ15L.Ku7Ivz9ML0aPUVBq2q9Z"

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

    withCredentials([file(credentialsId: JWT_KEY_CRED_ID, variable: 'jwt_key_file')]) {        
        stage('Deployment') {
            if (isUnix()) {//Para sistemas Unix el comando varía un poco el formato
                rc = sh returnStatus: true, script: "${sfdx} force:auth:logout --targetusername ${SFDC_USERNAME} -p" //Hacemos logout para evitar un error
				// Autorizamos la dev hub org
                rc = sh returnStatus: true, script: "${sfdx} force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${SFDC_USERNAME} --jwtkeyfile ${jwt_key_file} --setdefaultdevhubusername --instanceurl ${SFDC_HOST}"
            }else{//ejecutamos lo mismo para sistemas Windows
                rc = sh returnStatus: true, script:"\"${sfdx}\" force:auth:logout --targetusername ${SFDC_USERNAME} -p"
                rc = bat returnStatus: true, script: "\"${sfdx}\" force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${SFDC_USERNAME} --jwtkeyfile \"${jwt_key_file}\" --setdefaultdevhubusername --instanceurl ${SFDC_HOST}"
            }
            if (rc != 0) { error 'Org authorization has failed' }

			println rc

			  
            printf rmsg
            println(rmsg)
        }
    }
}