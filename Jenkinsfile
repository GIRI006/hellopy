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
                    dockerImage = docker.build(registry, '-t latest -t 1.0.0')
                }
            }
        }
        stage('Upload Image') {
            steps {
                script {
                    docker.withRegistry('', registryCredential) {
                        dockerImage.push('latest')
                        dockerImage.push('1.0.0')
                    }
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
