node {
    def newApp
    def registry = 'https://registry-1.docker.io/v2/'
	  def imagename = "frenzy669/roy"
    def registryCredential = 'frenzy669'
	
	stage('Git') {
		git 'https://github.com/roym44/MultibranchPipeline.git'
	}
	stage('Build') {
		nodejs(nodeJSInstallationName: 'NodeJS') { 
			sh label: '', script: '''
			cd src
			npm audit fix
			npm install --only=dev
			'''
			}
	}
	stage('Test') {
		nodejs(nodeJSInstallationName: 'NodeJS') { 
			sh label: '', script: '''
			npm test
		'''
		}
	}
	stage('Building image') {
        docker.withRegistry( registry, registryCredential ) {
		    def buildName = imagename + ":$BUILD_NUMBER"
			newApp = docker.build(buildName)
			newApp.push();
        }
	}
}