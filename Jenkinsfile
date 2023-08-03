pipeline{
    agent any
    
    stages{
        stage("Code"){
            steps{
                echo "Cloning the code"
                git url:"https://github.com/vinodbhuruk/django-notes-app.git",branch: "main"
                }
        }
    
        stage("Build"){
            steps{
            echo "buiding the code"
            sh "docker build -t notes-app ."
            }
        }
        
        stage("push the image to docker Hub"){
            steps{
                echo "pushing the image to docker Hub"
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerhubpass",usernameVariable:"dockerhubuser")]){
                sh "docker tag notes-app ${env.dockerhubuser}/notes-app:latest"
                sh "docker login -u ${env.dockerhubuser} -p ${env.dockerhubpass}"
                sh "docker push ${env.dockerhubuser}/notes-app:latest"
                }
            }
        }
        
        stage("Deploy"){
            steps{
                echo "Deploy the container on aws"
                sh "docker-compose down && docker-compose up -d"
                
            }
        }
    }
    
}