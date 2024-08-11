pipeline {
  agent none

  stages {
    stage('result package'){
      agent any
      when{
        changeset "**/result/**"
      }
      steps {
        echo 'Packaging result app with Docker'
        script {
          docker.withRegistry('https://index.docker.io/v1/','dockerlogin') {
            def imageName = "matheusmbar/result:v${env.BUILD_ID}"
            echo "Image name: ${imageName}"
            def voteImage = docker.build("${imageName}", "./result")
            voteImage.push()
            voteImage.push("latest")
          }
        }
      }
    }
    stage('vote package'){
      agent any
      when{
        changeset "**/vote/**"
      }
      steps {
        echo 'Packaging vote app with Docker'
        script {
          docker.withRegistry('https://index.docker.io/v1/','dockerlogin') {
            def imageName = "matheusmbar/vote:v${env.BUILD_ID}"
            echo "Image name: ${imageName}"
            def voteImage = docker.build("${imageName}", "./vote")
            voteImage.push()
            voteImage.push("latest")
          }
        }
      }
    }
    stage('worker package'){
      agent any
      when{
        changeset "**/worker/**"
      }
      steps {
        echo 'Packaging worker app with Docker'
        script {
          docker.withRegistry('https://index.docker.io/v1/','dockerlogin') {
            def imageName = "matheusmbar/worker:v${env.BUILD_ID}"
            echo "Image name: ${imageName}"
            def workerImage = docker.build("${imageName}", "./worker")
            workerImage.push()
            workerImage.push("latest")
          }
        }
      }
    }
    stage('deploy to dev'){
       agent any
       when{
         branch 'master'
       }
       steps{
         echo 'Deploy instavote app with docker compose'
         sh 'docker-compose up -d'
       }
     }
  }
  post {
    always {
      echo 'Building multibranch pipeline for application is completed..'
    }
  }
}
