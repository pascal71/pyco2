node {
	stage('Checkout') {
		checkout scm
	}
	stage('Build Container') {
		docker.build('${JOB_NAME}', '-f Dockerfile .')
	}
	stage('Write properties') {
	    sh "> spinnaker.properties"
	    sh "echo 'JOB_NAME=${JOB_NAME}' >> spinnaker.properties"
	    sh "echo 'BUILD_ID=${BUILD_ID}' >> spinnaker.properties"
	    archiveArtifacts artifacts: 'spinnaker.properties', fingerprint: true
	}
	stage('Push to ECR') {
		docker.withRegistry('https://465848653329.dkr.ecr.us-west-2.amazonaws.com', 'ecr:us-west-2:spinnaker-admin-aws') {
			docker.image('${JOB_NAME}').push('${BUILD_ID}')
	   }
	}
}
