pipeline {
    agent any;
    stages {
        stage ("code"){
            steps {
                git url: "https://github.com/SnehaSanam/two-tier-flask-app.git", branch: "master"
                }
        }
        stage ("Build") {
            steps {
                sh "docker build -t online-shop ."
            }
        }
        stage ("Docker push"){ 
            steps {
                withCredentials([usernamePassword(
                credentialsId:"dockerHubCreds",
                passwordVariable: "dockerHubPass", 
                usernameVariable: "dockerHubUser"
                )]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker image tag  trainwithshubham/two-tier-flask-app:latest ${env.dockerHubUser}/two-tier-flask-app:latest"
                sh "docker push ${env.dockerHubUser}/two-tier-flask-app:latest"
                }
            
            }
        }
        stage ("Deploy") {
            steps {
                sh "docker compose up -d"
            }
        }
    }
}
