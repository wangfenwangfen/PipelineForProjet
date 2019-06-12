node("master"){
		stage("push to nexus")
		bat 'cd C:/Program Files (x86)/Jenkins/workspace/SpringbootProjet/controller/target/'
		bat 'dir'
		nexusArtifactUploader(
			credentialsId: 'nexuspass', 
			groupId: 'com.tahoecn', 
			nexusUrl: 'localhost:8081', 
			nexusVersion: 'nexus3', 
			protocol: 'http', 
			repository: 'pipelinforprojet', 
			version: '1.0',
			artifacts: [
				[artifactId: 'controller', classifier: '', file: 'C:/Program Files (x86)/Jenkins/workspace/SpringbootProjet/controller/target/controller-1.0-SNAPSHOT.jar', type: 'jar']
			]
		)
	}
