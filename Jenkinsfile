@Library('nodesharelib') _

pipeline {

  environment {
    dockerimagename = "varmadocker26/nodeapp"
    dockerImage = ""
  }

  agent any

  stages {
    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }

    stage('Pushing Image') {
      environment {
               registryCredential = 'dockerhublogin'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }

    
     stage('Scan Containers') {
      steps {
        script {
          def var1 = load "Test.groovy"
          var1.build("Container Scan")
        }
      }
    }

    stage('Deploying App to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "deploymentservice.yml", kubeconfigId: "kubernetes")
        }
      }
    }

  }

}