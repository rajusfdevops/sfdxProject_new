#!groovy
import groovy.json.JsonSlurperClassic
node {

    def BUILD_NUMBER=env.BUILD_NUMBER
    def RUN_ARTIFACT_DIR="tests/${BUILD_NUMBER}"
   

    def HUB_ORG=env.HUB_ORG_DH
    def SFDC_HOST = env.SFDC_HOST_DH
    def JWT_KEY_CRED_ID = env.JWT_CRED_ID_DH
    def CONNECTED_APP_CONSUMER_KEY=env.CONNECTED_APP_CONSUMER_KEY_DH

    println 'KEY IS' 
    println JWT_KEY_CRED_ID
    println HUB_ORG
    println SFDC_HOST
    println CONNECTED_APP_CONSUMER_KEY
    println BUILD_NUMBER
    println RUN_ARTIFACT_DIR
   
    def toolbelt = tool 'toolbelt'
    println toolbelt
    stage('checkout source') {
        // when running in multi-branch job, one must issue this command
        checkout scm
    }

    withCredentials([file(credentialsId: JWT_KEY_CRED_ID, variable: 'jwt_key_file')]) {
        stage('Deploye Code') {
            if (isUnix()) {
                rc = sh returnStatus: true, script: "sfdx force:auth:jwt:grant --clientid 3MVG9wt4IL4O5wvLNlZtiBcMUlAYt4GuNK7bisXDGGOfAuTwSuWNdpJzSivlMXl9HKLERGxntRjfcQxms90P9 --jwtkeyfile server.key --username rajukumarsfdevops@gmail.com --instanceurl https://login.salesforce.com --setdefaultdevhubusername"
            }else{
                 rc = bat returnStatus: true, script: "sfdx force:auth:jwt:grant --clientid 3MVG9wt4IL4O5wvLNlZtiBcMUlAYt4GuNK7bisXDGGOfAuTwSuWNdpJzSivlMXl9HKLERGxntRjfcQxms90P9 --jwtkeyfile server.key --username rajukumarsfdevops@gmail.com --instanceurl https://login.salesforce.com --setdefaultdevhubusername"
            }
            if (rc != 0) { error 'hub org authorization failed' }

			println rc
			
			// need to pull out assigned username
			if (isUnix()) {
				rmsg = sh returnStdout: true, script: "sfdx force:mdapi:deploy -d manifest/. -u ${HUB_ORG}"
			}else{
			   rmsg = bat returnStdout: true, script: "sfdx force:mdapi:deploy -d manifest/. -u ${HUB_ORG}"
			}
			  
            printf rmsg
            println('Hello from a Job DSL script!')
            println(rmsg)
        }
    }
    
}
