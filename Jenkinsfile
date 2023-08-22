pipeline {
   agent any
   environment {
       registry = "girisatapathy/hello-world"
       registryCredential = 'docker_hub'
      // dockerImageTag = '${env.BUILD_ID}' // Specify the new tag for your image here
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
                   // Define the image name with the new tag
                   def dockerImage = docker.build("${registry}:{build-number})

                   // Push the new image to the Docker registry
                   docker.withRegistry('', registryCredential) {
                       dockerImage.push()
                   }

                   // Remove the previous image with an older tag
                   def previousTag = '0.9.0' // Replace with the tag you want to remove
                   sh "docker rmi ${registry}:${previousTag}"
               }
           }
       }
       stage('Run Docker Image') {
           steps {
               script {
                   docker.withRegistry('https://index.docker.io/v1/') {
                       docker.image('girisatapathy/helloworldpy').run()
                   }
               }
           }
       }
   }
}

