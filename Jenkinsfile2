node {
    def gitcommit
    stage('Verificación SCM') {
        checkout scm
        sh "git rev-parse --short HEAD > .git/commit-id"                        
        gitcommit = readFile('.git/commit-id').trim()
      }
    stage('test') {
        def containertest = docker.image('node:lts-hydrogen')
        containertest.pull()
        containertest.inside {
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