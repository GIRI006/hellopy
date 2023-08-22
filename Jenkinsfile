pipeline {

    agent any

    environment {

        registry = "girisatapathy/hello-world"

        registryCredential = 'docker_hub'

        dockerImageTag = '1.0.0' // Specify the new tag for your image here

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

                    

                    def dockerImage = docker.build("${registry}:${dockerImageTag}")


                    

                    docker.withRegistry('', registryCredential) {

                        dockerImage.push()

                    }


                    

                    def previousTag = 'latest'

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
