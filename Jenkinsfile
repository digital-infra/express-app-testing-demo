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
        }
    }
    stage('Docker build & Push'){
        docker.withRegistry('http://46.101.246.61:8070/library', 'harborcreds'){
            def app = docker.build("express-app-testing-demo:${commit_id}",".").push()
        }
    }
}
