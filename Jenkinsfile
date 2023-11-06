pipeline{
    agent {label "dev-agent"}
    stages{
        stage("code"){
            steps{
                echo "cloning the code....."
                git url: "https://github.com/mrunali2391/django-notes-app.git", branch: "main"
            }
        }
        
        stage("build"){
            steps{
                sh "docker build . -t note-app"
            }
        }
        stage("push to dockerhub"){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                    sh "docker tag note-app ${env.dockerHubUser}/note-app:latest"
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker push ${env.dockerHubUser}/note-app:latest"
                }
            }
        }
        stage("deploy"){
            steps{
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
