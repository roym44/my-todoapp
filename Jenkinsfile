node {
   def commit_id
   stage('GIT Checkout') {
     checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/roym44/todoapp.git']]])
     sh "git rev-parse --short HEAD > .git/commit-id"                        
     commit_id = readFile('.git/commit-id').trim()
   }
   stage('test') {
     nodejs(nodeJSInstallationName: 'nodejs') {
        sh label: '', script: '''
        npm install --only=dev
        npm test 
        '''
     }

   }
   stage('docker build/push') {
     docker.withRegistry('https://index.docker.io/v1/', 'roym44') {
       def app = docker.build("roym44/todoapp:${commit_id}", 'basics').push()
     }
   }
   stage('docker run') {
     sh label: '', script: """
      docker run --rm -tid --name docker_test -p 3000 roym44/todoapp:${commit_id}
      docker kill docker_test
      """
     }
   }
