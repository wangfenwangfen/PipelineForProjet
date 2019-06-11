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
		
		stage ('Artifactory configuration') {
            steps {
                rtServer (
                    id: "ARTIFACTORY_SERVER",
                    url: NEXUS_URL,
                    credentialsId: NEXUS_CREDENTIAL_ID
                )

                rtMavenDeployer (
                    id: "MAVEN_DEPLOYER",
                    serverId: "ARTIFACTORY_SERVER",
                    releaseRepo: NEXUS_REPOSITORY,
                    snapshotRepo: NEXUS_REPOSITORY
                )

                rtMavenResolver (
                    id: "MAVEN_RESOLVER",
                    serverId: "ARTIFACTORY_SERVER",
                    releaseRepo: NEXUS_REPOSITORY,
                    snapshotRepo: NEXUS_REPOSITORY
                )
            }
        }

        stage ('Exec Maven') {
            steps {
                rtMavenRun (
                    tool: "maven3.6.1",
                    pom: 'pom.xml',
                    goals: 'clean install',
                    deployerId: "MAVEN_DEPLOYER",
                    resolverId: "MAVEN_RESOLVER"
                )
            }
        }

			
		stage ('Publish to Nexus') {
            steps {
                rtPublishBuildInfo (
                    serverId: "ARTIFACTORY_SERVER"
                )
            }
        }
	}
}
