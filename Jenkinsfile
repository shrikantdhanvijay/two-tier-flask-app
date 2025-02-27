@Library("shared") _
pipeline{
    agent { label "dev"};
    // agent any;
    stages{
        stage("Code"){
            steps{
                echo "project clone karna hai"
                // git url:"https://github.com/shrikantdhanvijay/two-tier-flask-app.git", branch: "master"
                
                script{
                       clone.call("https://github.com/shrikantdhanvijay/two-tier-flask-app.git", "master")
               }
              echo "project clone ho gaya hai"
            }
        }

        stage("Trivy File System Scan"){
            steps{
              echo "file scan karna hai"
                // sh "trivy fs . -o results.json"
                script{
                    clone.trivy_fs()
                }
                echo "File scan ho gayi hai"
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
                echo "Push karna hai"   
                 script{
                    docker_push("shrikant-docker-hub-cred","two-tier-flask-app")
                }  
               echo "Push ho gaya hai"   
                // withCredentials([usernamePassword(
                //     credentialsId:"shrikant-docker-hub-cred",
                //     passwordVariable: "dockerHubPass",
                //     usernameVariable: "dockerHubUser"
                // )]){
                    
                //     echo "image push karna hai"
                //     sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                //     sh "docker image tag my-first-jenkins-project ${env.dockerHubUser}/two-tier-flask-app"
                //     sh "docker push ${env.dockerHubUser}/two-tier-flask-app:latest"
                //     echo "image push ho gayi"
                // }
            }
        }
        
        stage("Deploy"){
            steps{
                echo "Project deploy karna hai"
                sh "docker compose up -d --build flask-app"
            }
        }
    }
    
post{
        success{
            script{
                emailext from: 'shrikantdhanvijay358@gmail.com',
                to: 'shreedhanvijay08@gmail.com',
                body: 'Build success for Demo CICD App',
                subject: 'Build success for Demo CICD App'
            }
        }
        failure{
            script{
                emailext from: 'shrikantdhanvijay358@gmail.com',
                to: 'shreedhanvijay08@gmail.com',
                body: 'Build Failed for Demo CICD App',
                subject: 'Build Failed for Demo CICD App'
            }
        }
    }
}
