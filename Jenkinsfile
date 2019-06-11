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
			
        stage("publish to nexus") {
            steps {
                script {
                    // Read POM xml file using 'readMavenPom' step , this step 'readMavenPom' is included in: https://plugins.jenkins.io/pipeline-utility-steps
                    pom = readMavenPom file: "pom.xml";
                    // Find built artifact under target folder
                    filesByGlob = findFiles(glob: "target/*.${pom.packaging}");
                    // Print some info from the artifact found
                    echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
                    // Extract the path from the File found
                    artifactPath = filesByGlob[0].path;
                    // Assign to a boolean response verifying If the artifact name exists
                    artifactExists = fileExists artifactPath;
                    if(artifactExists) {
                        echo "*** File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}";
                        nexusArtifactUploader(
                            nexusVersion: NEXUS_VERSION,
                            protocol: NEXUS_PROTOCOL,
                            nexusUrl: NEXUS_URL,
                            groupId: pom.groupId,
                            version: pom.version,
                            repository: NEXUS_REPOSITORY,
                            credentialsId: NEXUS_CREDENTIAL_ID,
                            artifacts: [
                                // Artifact generated such as .jar, .ear and .war files.
                                [artifactId: pom.artifactId,
                                classifier: '',
                                file: artifactPath,
                                type: pom.packaging],
                                // Lets upload the pom.xml file for additional information for Transitive dependencies
                                [artifactId: pom.artifactId,
                                classifier: '',
                                file: "pom.xml",
                                type: "pom"]
                            ]
                        );
                    } else {
                        error "*** File: ${artifactPath}, could not be found";
                    }
                }
            }
        }
    }
