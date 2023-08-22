pipeline {
   agent any
   environment {
       registry = "girisatapathy/hello-world"
       registryCredential = 'docker_hub'
       dockerImageTag = "${BUILD_NUMBER}"
       previousImageTag = "${BUILD_NUMBER - 1}"
   }
   stages {
       stage('Cloning Git') {
           steps {
               checkout scm
           }
       }
       stage('Building and Pushing Docker Image') {
           steps {
               script {
                   // Build the Docker image with the build number as the tag
                   def dockerImage = docker.build("${registry}:${dockerImageTag}")

                   // Push the image to the Docker registry
                   docker.withRegistry('', registryCredential) {
                       dockerImage.push()
                   }

                   // Remove the previous image with the old tag from your local machine
                   sh "docker rmi ${registry}:${previousImageTag}" // Replace with appropriate logic
               }
           }
       }
       stage('Run Docker Image') {
           steps {
               script {
                   // Run a container based on the specified image tag
                   docker.withRegistry('https://index.docker.io/v1/') {
                       docker.image("${registry}:${dockerImageTag}").run()
                   }
               }
           }
       }
   }
}



