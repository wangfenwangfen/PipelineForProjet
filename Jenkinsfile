pipeline {
  agent any
  stages {
    stage('RecupererRessource') {
      steps {
        git(url: 'https://github.com/wangfenwangfen/PipelineForProjet.git', branch: 'master')
      }
    }
    stage('build') {
      steps {
        bat 'build.bat'
      }
    }
  }
}