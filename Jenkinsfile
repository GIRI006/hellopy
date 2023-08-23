pipeline {
   agent any

   environment {
       registry = "girisatapathy/hello-world"
       registryCredential = 'docker_hub'
       dockerImageTag = "${BUILD_NUMBER}"
       kubeconfig = credentials('kube_cluster')
       namespace = 'jenkins'
       deploymentName = 'helloworldpy'
       manifestFile = '/home/ubuntu/hellopy/helloworld.yaml' 
   }

   stages {
       stage('Cloning Git') {
           steps {
               checkout scm
           }
       }

       stage('Determine Previous Image Tag') {
           steps {
               script {
                   def previousTagFile = readFile("/var/lib/jenkins/workspace/hellopy/previous_docker_image_tag.txt")
                   previousImageTag = previousTagFile.trim()
               }
           }
       }

       stage('Building and Pushing Docker Image') {
           steps {
               script {
                   def dockerImage = docker.build("${registry}:${dockerImageTag}")
                   docker.withRegistry('', registryCredential) {
                       dockerImage.push()
                   }
                   sh "docker rmi -f ${registry}:${previousImageTag}"
               }
           }
       }

       stage('Deploy to Kubernetes') {
           steps {
               script {
                   def kubeconfigPath = "/home/ubuntu/.kube/config"
                   writeFile file: kubeconfigPath, text: kubeconfig
                   

                   
                   sh "kubectl apply -f ${manifestFile} -n ${namespace}"
               }
           }
       }
   }

   post {
       always {
           writeFile file: "/var/lib/jenkins/workspace/hellopy/previous_docker_image_tag.txt", text: "${dockerImageTag}"
       }
   }
}

