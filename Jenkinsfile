pipeline {
   agent any
   environment {
       registry = "girisatapathy/hello-world"
       registryCredential = 'docker_hub'
       // Use BUILD_NUMBER environment variable as the Docker image tag
       dockerImageTag = "${BUILD_NUMBER}"
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
