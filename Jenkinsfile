pipeline {
    agent{
      label "dev"
}
    stages {
        stage("Code"){ 
            steps{
             echo "Cloning Code"
             git url:"https://github.com/kishor-645/django-notes-app.git", branch: "main"
            }
        }
        stage("Build"){
         steps{
             echo "Build Code"
             sh "docker build -t django-note-app ."             
         }   
        }
        stage("Push to docker hub"){
         steps{
            echo "Push on Docker hub Code"
            withCredentials([usernamePassword(credentialsId: 'dockid', usernameVariable: 'docker_user', passwordVariable: 'docker_pass')]) { 
            sh "docker tag django-note-app ${env.docker_user}/django-note-app:latest"
            sh "docker login -u ${env.docker_user} -p ${env.docker_pass}"
            sh "docker push ${env.docker_user}/django-note-app:latest"
            }
         }   
        }
        stage("Deploy"){
         steps{
            echo "Deploy Code on webserver"
            sh "docker-compose down && docker-compose up -d "
             
         }    
        }
    }
}
