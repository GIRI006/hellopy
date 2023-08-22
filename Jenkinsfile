pipeline {
    agent any
    environment {
        registry = "girisatapathy/hello-world"
        registryCredential = 'docker_hub'
        dockerImage = ''
    }
    stages {
        stage('Cloning Git') {
            steps {
                checkout scm
            }
        }
        stage('Building image') {
            steps {
                script {
                    dockerImage = docker.build(registry)
                }
            }
        }
        stage('Upload Image') {
            steps {
                script {
                    docker.withRegistry('', registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Run Docker Image') {
            steps {
                script {
                    docker.run(image: dockerImage)
                }
            }
        }
    }
}
