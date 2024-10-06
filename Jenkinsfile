pipeline {
    agent {
        node {
            label "dev"
        }
    }
    stages {
        stage("code clone"){
            steps{
                git url :"https://github.com/shaiferozbasha/django.git", branch: "main"
            }
        }
        stage("Build & test"){
            steps{
                sh "docker build . -t notes-app-jenkins:latest"
            }
        }
        stage("push to dockerhub"){
            steps{
                withCredentials(
                [usernamePassword(
                credentialsId:"dockerCreds",
                usernameVariable:"dockerHubUser",
                passwordVariable:"dockerHubPass"
)
]
){      
            sh "docker image tag notes-app-jenkins:latest ${env.dockerHubUser}/notes-app-jenkins:latest"
            sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
            sh "docker push ${env.dockerHubUser}/notes-app-jenkins:latest"
            }
            }
        }
        stage("Deploy"){
            steps{
                echo "This is deploying the code"
                sh "docker compose down && docker compose up -d"
            }
        }
    }
}

