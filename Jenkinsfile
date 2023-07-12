pipeline{
    agent any
    
    stages {
        stage("Code"){
           steps{
               echo "Cloing the code"
               git url: "https://github.com/Pankaj-Surya/django-notes-app.git", branch: "main"
           } 
        }
        stage("Build"){
            steps{
                echo "Building code"
                sh "docker build -t my-note-app ."
            }
        }
        stage("Push to Dockerhub"){
            steps{
                echo "Push to DockerHub"
                withCredentials([usernamePassword(credentialsId: 'dockerHub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                // Code or steps that require the credentials
                sh "docker tag my-note-app ${env.USERNAME}/my-note-app:latest"
                sh "docker login -u ${env.USERNAME} -p ${env.PASSWORD}"
                sh "docker push ${env.USERNAME}/my-note-app:latest"
                }
                
            }
        }
        stage("Deploying the Container"){
            steps{
                echo "Deploy"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
