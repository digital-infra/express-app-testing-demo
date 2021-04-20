node{
    def commit_id
    stage("Preperation"){
        checkout scm
        sh "ls -l"
        sh "git rev-parse --short HEAD > .git/commit-id"
        commit_id = readFile('.git/commit-id').trim()
    }
    stage('Test'){
        def myContainer = docker.image("node")
        myContainer.pull()
        myContainer.inside{
            sh 'npm install'
            sh 'npm test'
        }
    }
    stage('Docker build & Push'){
        docker.withServer('172.20.10.2:2376'){
            def app = docker.build("express-app-testing-demo:${commit_id}",".").push()
        }
    }
}
