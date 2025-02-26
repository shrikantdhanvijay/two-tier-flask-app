pipeline{
    agent { label "dev"};
    
    stages{
        stage("Code"){
            steps{
                echo "project clone karna hai"
                git url:"https://github.com/shrikantdhanvijay/two-tier-flask-app.git", branch: "master"
                echo "project clone ho gaya hai"
            }
        }
        
        stage("Build"){
            steps{
                echo "Projetct build karna hai"
                sh "docker build -t my-first-jenkins-project ."
                echo "Build ho gaya hai"
            }
        }
        
        stage("Test"){
            steps{
                echo "Test karna hai"   
            }
        }
        
        stage("Push to Docker hub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId:"dockerHubCreds",
                    passwordVariable: "dockerHubPass",
                    usernameVariable: "dockerHubUser"
                )]){
                    
                    echo "image push karna hai"
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker image tag my-first-jenkins-project ${env.dockerHubUser}/two-tier-flask-app"
                    sh "docker push ${env.dockerHubUser}/two-tier-flask-app:latest"
                    echo "image push ho gayi"
                }
            }
        }
        
        stage("Deploy"){
            steps{
                echo "Project deploy karna hai"
                sh "docker compose up -d --build flask-app"
            }
        }
    }
}
