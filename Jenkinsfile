#!/usr/bin/groovy

def jenkinsFile
stage('Loading Jenkins file') {
  jenkinsFile = fileLoader.fromGit('Jenkinsfile', 'https://github.com/KubeBitPoc/webapp.git', 'master', null, '')
}

jenkinsFile.start()


