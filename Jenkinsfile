#!groovy

node {

    def SF_CONSUMER_KEY=env.HCP_SF_CONSUMER_KEY
    def SF_USERNAME=env.HCP_SF_USERNAME
    def SERVER_KEY_CREDENTIALS_ID=env.HCP_SERVER_KEY_CREDENTIALS_ID
    def DEPLOYDIR='src'
    def TEST_LEVEL='RunLocalTests'
    def SF_INSTANCE_URL = env.HCP_SF_INSTANCE_URL ?: "https://test.salesforce.com"

<<<<<<< HEAD
    def toolbelt = tool 'toolbelt'

=======
    echo "step 1"
    def toolbelt = tool 'toolbelt'


>>>>>>> 919a6abe901e204760d36c8e79fed59fde2ab32c
    // -------------------------------------------------------------------------
    // Check out code from source control.
    // -------------------------------------------------------------------------

    stage('checkout source') {
        checkout scm
    }
 
<<<<<<< HEAD
=======
    echo "step 2"
>>>>>>> 919a6abe901e204760d36c8e79fed59fde2ab32c
    // -------------------------------------------------------------------------
    // Run all the enclosed stages with access to the Salesforce
    // JWT key credentials.
    // -------------------------------------------------------------------------

 	withEnv(["HOME=${env.WORKSPACE}"]) {	
<<<<<<< HEAD
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

	    }
	}
}

=======
	    echo "step 3.1"
		echo "${SF_CONSUMER_KEY}"
		echo "${SF_USERNAME}"
		echo "${SERVER_KEY_CREDENTIALS_ID}"
		echo "${toolbelt}"
		echo "${SF_INSTANCE_URL}"
	    withCredentials([file(credentialsId: SERVER_KEY_CREDENTIALS_ID, variable: 'server_key_file')]) {
		// -------------------------------------------------------------------------
		// Authenticate to Salesforce using the server key.
		// -------------------------------------------------------------------------
                echo "step 3.2"
		stage('Authorize to Salesforce') {
			rc = command "${toolbelt}/sfdx force:auth:jwt:grant --instanceurl ${SF_INSTANCE_URL} --clientid ${SF_CONSUMER_KEY} --jwtkeyfile ${server_key_file} --username ${SF_USERNAME} --setalias UAT"
		    if (rc != 0) {
			error 'Salesforce org authorization failed.'
		    }
			echo "step 3.3"
		}


		// -------------------------------------------------------------------------
		// Deploy metadata and execute unit tests.
		// -------------------------------------------------------------------------

		stage('Deploy and Run Tests') {
		    rc = command "${toolbelt}/sfdx force:mdapi:deploy --wait 10 --deploydir ${DEPLOYDIR} --targetusername UAT --testlevel ${TEST_LEVEL}"
		    if (rc != 0) {
			error 'Salesforce deploy and test run failed.'
		    }
			echo "step 3.4"
		}


		// -------------------------------------------------------------------------
		// Example shows how to run a check-only deploy.
		// -------------------------------------------------------------------------

		//stage('Check Only Deploy') {
		//    rc = command "${toolbelt}/sfdx force:mdapi:deploy --checkonly --wait 10 --deploydir ${DEPLOYDIR} --targetusername UAT --testlevel ${TEST_LEVEL}"
		//    if (rc != 0) {
		//        error 'Salesforce deploy failed.'
		//    }
		//}
	    }
	}
}
echo "step 3.5"
>>>>>>> 919a6abe901e204760d36c8e79fed59fde2ab32c
def command(script) {
    if (isUnix()) {
        return sh(returnStatus: true, script: script);
    } else {
		return bat(returnStatus: true, script: script);
    }
}
