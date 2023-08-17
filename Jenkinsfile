pipeline {
   agent any

   stages {
       stage('Checkout') {
           steps {
               
               git 'https://github.com/GIRI006/hellopy.git'
           }
       }

       stage('Build and Dockerize') {
           steps {
               
               sh 'docker build -t hello-world .'
           }
       }

       stage('Run in Docker') {
           steps {
               
               sh 'docker run hello-world'
           }
       }
   }
}
