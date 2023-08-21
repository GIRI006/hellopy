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
        stage('Deploy to k8s') {
            steps {
                script {
                    def kubeConfig = readFile("/home/ec2-user/.kube/config")
                    writeFile file: 'kubeconfig', text: kubeConfig
                    kubeConfig = readTrusted('kubeconfig')
                    kubernetesDeploy kubeconfigContent: kubeConfig, configs: 'hello.yaml'
                }
            }
        }
    }
}
