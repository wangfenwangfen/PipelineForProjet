pipeline {
	//agent any  (peu import quel agent noeud)
	
	//ou bien spécifier un agent par son label 
    agent {
		
		label "master"
    }
    tools {
        // Note: this should match with the tool name configured in your jenkins instance, ici nomme "maven3.6.1" (JENKINS_URL/configureTools/)
        maven "maven3.6.1"
    }
    environment {
        // This can be nexus3 or nexus2
        NEXUS_VERSION = "nexus3"
        // This can be http or https
        NEXUS_PROTOCOL = "http"
        // Where your Nexus is running (172.17.0.3:8081 ou bien fr.xxxxx:8081)
        NEXUS_URL = "localhost:8081"
        // Repository where we will upload the artifact ici exemple name repo est pipelinforprojet
        NEXUS_REPOSITORY = "pipelinforprojet"
        // Jenkins credential id to authenticate to Nexus OSS (credential id creer dans jenkins avec son login mdp nexus)
        NEXUS_CREDENTIAL_ID = "nexuspass"
    }

	//les etatpes stages en ordre
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
		
		stage ('configurer artifact') {
            steps {
			script {
				def ARTIFACTORY_SERVER = Artifactory.newServer url:  NEXUS_URL, credentialsId: NEXUS_CREDENTIAL_ID, timeout = 300
			}				
				
            }
        }
		
		stage ('push to nexus'){
			steps {
				script{
					def uploadSpec = """{
						"files": [{
						"pattern": "C:/Program Files (x86)/Jenkins/workspace/SpringbootProjet/controller/target/controller-1.0-SNAPSHOT.jar",
						"target": NEXUS_REPOSITORY
							}
						]
					}"""

					server.upload(uploadSpec)
				}
			}
		
		}
			
	}
}
