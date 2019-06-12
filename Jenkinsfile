pipeline {
	//any agent for all stages, use label can speficier agent
	agent any
	
	tools {
        // Note: this should match with the tool name configured in your jenkins instance, ici nomme "maven3.6.1" (JENKINS_URL/configureTools/)
        maven "maven3.6.1"
    }
	
	stages {
		stage('RecupererCodeSource') {
			steps {
				//clone the source from git
				git(url: 'https://github.com/wangfenwangfen/PipelineForProjet.git', branch: 'master')
			}
		}
		
		stage('mvnbuild') {
			steps {
				//here use windows bat,if use unix shell, use script step
				bat 'mvn clean install'
			}
		}
		
		stage("push to nexus"){
		steps{
			bat 'cd C:/Program Files (x86)/Jenkins/workspace/SpringbootProjet/controller/target/'
			bat 'dir'
			nexusArtifactUploader(
				credentialsId: 'nexuspass', 
				groupId: 'fr.fw', 
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
			
		}
	}	
}
