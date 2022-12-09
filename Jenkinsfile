node {
   def gitcommit
   stage('Verificación SCM') {
     checkout scm
     sh "git rev-parse --short HEAD > .git/commit-id"                        
     gitcommit = readFile('.git/commit-id').trim()
   }
   stage('test') {
     nodejs(nodeJSInstallationName: 'NodeJS') {
       sh 'npm install --only=dev'
       sh 'npm test'
     }
   }
   stage('Docker Build & Push') {
     docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-systtekcloud') {
      def nodejsapp = docker.build("systtekcloud/nodejsapp:${gitcommit}", ".")
      nodejsapp.push()
     }
   }
}