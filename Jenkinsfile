#!groovy
node {

	//Define gradle tool home. It is configured in the global tool configuration
    def gradleHome = tool 'gradle-2.13'
	stage 'Build Project Using Gradle'
	dir("${ProjectWorkspace}"){
		sh "${gradleHome}/bin/gradle assemble" //Execute the assemble task on project
	}
	
	stage 'Retrieving the Artifactort URL' //Retrieving the details of all artifacts uploaded to the URL
	httpRequest authentication: 'artifactory', consoleLogResponseBody: true, outputFile: 'repository.json', url: "${ArtifactoryURL}"
	
	stage 'Sending out Approval Request'  //Sending out the approval request email to the manager
	mail (to: "${ApproverEmail}",
         subject: "Job '${env.JOB_NAME}' (${env.BUILD_NUMBER}) is waiting for input",
         body: "Please go to ${env.BUILD_URL}.");
	def userInput = true
	def didTimeout = false
	try {
		timeout(time: 60, unit: 'SECONDS') { // Setting timeout for approval
			userInput =input (id: 'userInput', message: 'Application packagin done. Proceed with deployment?')
		}
	} catch(e){
		def user = e.getCauses()[0].getUser()
		if('SYSTEM' == user.toString()) {
			didTimeout = true
		} else {
			userInput = false
			echo "Aborted by: [${user}]"
		}
	}
	if (didTimeout) { //If there is a timeout do nothing
		echo "No input received.Please try again."
	}
	else if (!userInput){ //If the manager approves call the "Deployment" job
		build 'Deployment'
	} else {			// If not approved then fail the build
		echo "Not so good "
		currentBuild.result = 'FAILURE'
	}
}