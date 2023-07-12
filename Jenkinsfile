node {
    def app
    stage('Clone Repo') {
        checkout scm
    }

    stage('Build Image') {
        app = docker.build("x64nik/argo-flask-apps")
    }

    stage('Push Image') {
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            app.push("${env.BUILD_NUMBER}")
        }
    }

    stage('Trigger Manifest Update') {
                echo "triggering cicd-flask-test1-deploy job"
                build job: 'cicd-flask-test1-deploy', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
        }
}