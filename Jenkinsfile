pipeline {

    agent any

    environment {

        registry = "girisatapathy/hello-world"

        registryCredential = 'docker_hub'

        dockerImageTag = "${BUILD_NUMBER}"

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

        stage('Run Docker Image') {

            steps {

                script {

                    

                    docker.withRegistry('https://index.docker.io/v1/') {

                        docker.image("${registry}:${dockerImageTag}").run()

                    }

                }

            }

        }

    }

    post {

        always {

            // Store the current build's Docker image tag for the next build

            writeFile file: "/var/lib/jenkins/workspace/hellopy/previous_docker_image_tag.txt", text: "${dockerImageTag}"

        }

    }

}
