node {
  def registry = "girisatapathy/hello-world"
  def registryCredential = 'docker_hub'
  def dockerImageTag = BUILD_NUMBER
  def kubeconfig = credentials('kube_cluster')
  def namespace = 'jenkins'
  def deploymentName = 'helloworldpy'
  def manifestFile = '/home/ubuntu/hellopy/helloworld.yaml'
  def kubeconfigPath = "/home/ubuntu/.kube/config"

  stage('Cloning Git') {
    checkout scm
  }

  stage('Determine Previous Image Tag') {
    script {
      def previousTagFile = readFile("/var/lib/jenkins/workspace/hellopy/previous_docker_image_tag.txt")
      previousImageTag = previousTagFile.trim()
    }
  }

  stage('Building and Pushing Docker Image') {
    script {
      def dockerImage = docker.build("${registry}:${dockerImageTag}")
      docker.withRegistry('', registryCredential) {
        dockerImage.push()
      }
      sh "docker rmi -f ${registry}:${previousImageTag}"
    }
  }

  stage('Deploy to Kubernetes') {
    sh "kubectl --kubeconfig=${kubeconfigPath} apply -f ${manifestFile} -n ${namespace}"
  }

  stage('Post Build') {
    // Perform any post-build actions here
    writeFile file: "/var/lib/jenkins/workspace/hellopy/previous_docker_image_tag.txt", text: "${dockerImageTag}"
  }
}

