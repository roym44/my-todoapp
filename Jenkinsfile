node {
    def newApp
    def registry = 'https://registry-1.docker.io/v2/'
	def imagename = "roym44/multibranchpipeline"
    def registryCredential = 'roym44' // ID which is found in the credentials in Jenkins
	
	stage('Git') {
		git 'https://github.com/roym44/todoapp.git'
	}
	stage('Build') {
		nodejs(nodeJSInstallationName: 'nodejs') { 
			sh label: '', script: '''
			cd src
			npm audit fix
			npm install --only=dev
			'''
			}
	}
	stage('Test') {
		nodejs(nodeJSInstallationName: 'nodejs') { 
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