node {

    stage("Git Clone"){
            git branch: 'main', credentialsId: 'GIT_HUB_CREDENTIALS', url: 'https://github.com/x64nik/cicd-flask-test.git'
        }
    
    stage("Docker build"){
        sh 'docker version'
        sh 'docker build -t basic-flask-app-demo-01 .'
        sh 'docker image list'
        sh "docker tag basic-flask-app-demo-01 x64nik/basic-flask-app:${env.BUILD_NUMBER}"
    } 
    
    stage("Docker Login"){
        withCredentials([string(credentialsId: 'DOCKER_HUB_PASSWORD', variable: 'PASSWORD')]) {
            sh 'docker login -u x64nik -p $PASSWORD'
        }
    } 
    
    stage("Push Image to Docker Hub"){
        sh "docker push  x64nik/basic-flask-app:${env.BUILD_NUMBER}"
    }
       
    stage("test") {
        sh "cat deployment/k8s-deploy.yml"
    }
}