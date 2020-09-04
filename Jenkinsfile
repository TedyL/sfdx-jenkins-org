#!groovy

node {

    def SF_CONSUMER_KEY=env.HCP_SF_CONSUMER_KEY
    def SF_USERNAME=env.HCP_SF_USERNAME
    def SERVER_KEY_CREDENTIALS_ID=env.HCP_SERVER_KEY_CREDENTIALS_ID
    def DEPLOYDIR='src'
    def TEST_LEVEL='RunLocalTests'
    def SF_INSTANCE_URL = env.HCP_SF_INSTANCE_URL ?: "https://test.salesforce.com"

    def toolbelt = tool 'toolbelt'

    // -------------------------------------------------------------------------
    // Check out code from source control.
    // -------------------------------------------------------------------------

    stage('checkout source') {
        checkout scm
    }
 
    // -------------------------------------------------------------------------
    // Run all the enclosed stages with access to the Salesforce
    // JWT key credentials.
    // -------------------------------------------------------------------------

 	withEnv(["HOME=${env.WORKSPACE}"]) {	
	    withCredentials([file(credentialsId: SERVER_KEY_CREDENTIALS_ID, variable: 'server_key_file')]) {
            
            // -------------------------------------------------------------------------
            // Authenticate to Salesforce using the server key.
            // -------------------------------------------------------------------------
            stage('Authorize to Salesforce') {
                rc = command "${toolbelt}/sfdx force:auth:jwt:grant --instanceurl ${SF_INSTANCE_URL} --clientid ${SF_CONSUMER_KEY} --jwtkeyfile ${server_key_file} --username ${SF_USERNAME} --setalias Demo"
                if (rc != 0) {
                error 'Salesforce org authorization failed.'
                }
            }

            stage('Display Demo Org') {
                rc = command "${toolbelt}/sfdx force:org:display --targetusername Demo"
                if (rc != 0) {
                    error 'Salesforce demo org display failed.'
                }
            }

            // -------------------------------------------------------------------------
            // Deploy metadata and execute unit tests.
            // -------------------------------------------------------------------------
            stage('Deploy To Demo Org') {
                rc = command "${toolbelt}/sfdx force:source:deploy --manifest manifest/package.xml --testlevel ${TEST_LEVEL}"
                if (rc != 0) {
                    error 'Salesforce push to demo org failed.'
                }
            }
            /*
            // -------------------------------------------------------------------------
            // Deploy metadata and execute unit tests.
            // -------------------------------------------------------------------------
            stage('Display Demo Org') {
                rc = command "${toolbelt}/sfdx force:org:display --targetusername Demo"
                if (rc != 0) {
                    error 'Salesforce demo org display failed.'
                }
            }

            // -------------------------------------------------------------------------
            // Display demo org info.
            // -------------------------------------------------------------------------
            stage('Push To Demo Org') {
                rc = command "${toolbelt}/sfdx force:source:push --targetusername Demo"
                if (rc != 0) {
                    error 'Salesforce push to demo org failed.'
                }
            }
 
 
            // -------------------------------------------------------------------------
            // Run unit tests in demo org.
            // -------------------------------------------------------------------------
            stage('Run Tests In Demo Org') {
                rc = command "${toolbelt}/sfdx force:apex:test:run --targetusername Demo --wait 10 --resultformat tap --codecoverage --testlevel ${TEST_LEVEL}"
                if (rc != 0) {
                    error 'Salesforce unit test run in test scratch org failed.'
                }
            }
            */
	    }
	}
}

def command(script) {
    if (isUnix()) {
        return sh(returnStatus: true, script: script);
    } else {
		return bat(returnStatus: true, script: script);
    }
}
