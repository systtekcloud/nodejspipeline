node {
    def gitcommit
    stage('Verificación SCM') {
        checkout scm
        sh "git rev-parse --short HEAD > .git/commit-id"                        
        gitcommit = readFile('.git/commit-id').trim()
      }
    stage('test') {
        nodejs(nodeJSInstallationName: 'nodejs') {
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
post {
        always {
            echo 'This will always run'
        }
        success {
            echo 'This will run only if successful'
        }
        failure {
            echo 'This will run only if failed'
        }
        unstable {
            echo 'This will run only if the run was marked as unstable'
        }
        changed {
            echo 'This will run only if the state of the Pipeline has changed'
            echo 'For example, if the Pipeline was previously failing but is now successful'
        }
    }