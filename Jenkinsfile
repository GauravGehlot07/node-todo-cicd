pipeline{
  agent any
    stages{
      stage('code'){
        steps{
          git url: 'https://github.com/GauravGehlot07/node-todo-cicd', branch: 'master'
        }
      }
      stage('build'){
        steps{
          sh "docker build -t node-todo-cicd ."
        }
      }
      stage('push image to docker hub'){
        steps{
          withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
            sh "docker login -u ${env.DOCKER_USERNAME} -p ${env.DOCKER_PASSWORD}"
            sh "docker tag node-todo-cicd:latest ${env.DOCKER_USERNAME}/node-todo-cicd:latest"
            sh "docker push ${env.DOCKER_USERNAME}/node-todo-cicd:latest"
          }
        }
      }   
      stage("deploying to aws"){
        steps{
          sh "docker-compose down" 
          sh "docker-compose up -d"
        }
      }
    }
}
