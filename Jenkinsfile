pipeline {
   agent any

   stages {
       stage('Checkout') {
           steps {
               // Checking out the source code from version control (e.g., Git)
               git 'https://github.com/yourusername/hello-world.git'
           }
       }

       stage('Build and Dockerize') {
           steps {
               // Building a Docker image with the Hello World program
               sh 'docker build -t hello-world .'
           }
       }

       stage('Run in Docker') {
           steps {
               // Running the Docker container
               sh 'docker run hello-world'
           }
       }
   }
}
