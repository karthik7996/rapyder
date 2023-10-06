pipeline {
    agent any

    stages {
        stage('code_checkout') {
            steps {
                git branch: "$BranchName", credentialsId: 'username_passwd', url: 'https://gitlab.com/superzop/vehicle-routing.git'
            }
        }
        stage("build docker image") {
            steps {
                sh "docker build -t 143941255581.dkr.ecr.ap-south-1.amazonaws.com/vehicle-routing-prod:$BUILD_NUMBER ."
            }
        }
        stage("login ecr and push image") {
            steps{
                sh "aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 143941255581.dkr.ecr.ap-south-1.amazonaws.com"
                sh "docker push 143941255581.dkr.ecr.ap-south-1.amazonaws.com/vehicle-routing-prod:$BUILD_NUMBER"
            }
        }
        stage("Remove image from jenkins server"){  
           steps {
               sh "docker rmi 143941255581.dkr.ecr.ap-south-1.amazonaws.com/vehicle-routing-prod:$BUILD_NUMBER"

        }
    }
         stage ("update tag and push helm charts") {
          steps {
             script{
                  git branch: 'prod', credentialsId: 'username_passwd', url: 'https://gitlab.com/superzop/dev-sz-helm-chart.git'
                  sh "ls"
                  sh 'sed -i "s/tag: .*/tag: ${BUILD_NUMBER}/" ./vehicle/values.yaml'
                  sh "git add ./vehicle/values.yaml"
                  sh "git status"
                  sh 'git commit -m "CI/CD pipeline updated $IMAGE_TAG image to new image tag"'
                  sh 'git config --global credential.helper cache'
		          sh "git push origin prod" 
                }
            }
          
        }
        stage ("Deploy app in eks") {
          steps {
              sh "helm upgrade vehicle ./vehicle -n superzop"
          }
        }
    }
    post {
            success {
            script {
                slackSend(channel: "jenkins-production", message: "prod-sz-vehicle-routing-pipeline successful")
            }
        }
        failure {
            script {
                slackSend(channel: "jenkins-production", message: "prod-sz-vehicle-routing-pipeline failure")
            }
        }
        
        always {
            cleanWs()
        }
    }
}
