@Library("Shared") _
pipeline {
    agent {label "dev"};
    stages {
        stage ("Code"){
            steps {
                script{
                clone("https://github.com/SnehaSanam/two-tier-flask-app.git","master")
                }
        }
        }
        stage ("Build") {
            steps {
                sh "docker build -t two-tier-flask-app:latest ."
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
post{
        success{
            script{
                emailext from: 'snehassanam@gmail.com',
                to: 'snehassanam@gmail.com',
                body: 'Build success for Demo CICD App',
                subject: 'Build success for Demo CICD App'
            }
        }
        failure{
            script{
                emailext from: 'snehassanam@gmail.com',
                to: 'snehassanam@gmail.com',
                body: 'Build Failed for Demo CICD App',
                subject: 'Build Failed for Demo CICD App'
            }
        }
    }
}
