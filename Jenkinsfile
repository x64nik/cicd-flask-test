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
       
    stage("Update deployment file") {
        sh "cat deployment/k8s-deploy.yml"
        sh "sed -i 's+x64nik/basic-flask-app.*+x64nik/basic-flask-app:${env.BUILD_NUMBER}+g' deployment/k8s-deploy.yml"
        sh "cat deployment/k8s-deploy.yml"
    }

    stage('Update GIT') {
            script {
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    withCredentials([usernamePassword(credentialsId: 'GIT_HUB_CREDENTIALS', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                        //def encodedPassword = URLEncoder.encode("$GIT_PASSWORD",'UTF-8')
                        sh "git config user.email rushidarunte123@gmail.com"
                        sh "git config user.name x64nik"
                        sh "git checkout argocd"
                        sh "cat deployment/k8s-deploy.yml"
                        sh "git add ."
                        sh "git commit -m 'updated image tag to : ${env.BUILD_NUMBER}'"
                        sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/cicd-flask-test.git HEAD:argocd"
      }
    }
  }
}

}