pipeline{
    agent any 
    stages{
        stage("code"){
            steps{
                echo"clone the code"
                git url: "https://github.com/vinodbhuruk/django-notes-app.git", branch: "main"
            }
            
        }
        stage("Build"){
            steps{
            echo "buiding the code"
            sh "docker build -t notes-app ."
            }
        }
        stage("PUSH TO Docker Hub"){
             steps {
                echo "Pushing the image to docker hub"
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerhubPass",usernameVariable:"dockerhubUser")]){
                sh "docker tag notes-app ${env.dockerhubUser}/notes-app:latest"
                sh "docker login -u ${env.dockerhubUser} -p ${env.dockerhubPass}"
                sh "docker push ${env.dockerhubUser}/notes-app:latest"
                }
            }
            
        }
        stage("Deploy"){
             steps{
                echo"deploy container on server"
                sh "docker-compose down && docker-compose up -d"
            }
            
        }
    }
}
