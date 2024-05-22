node {
  def dockerHubRepo = 'becka.kg99@gmail.com/pwoakk'
  def dockerHubCredentialsId = 'DockerHub_ID'

  stage("Clone the project") {
    git branch: 'master', url: 'https://github.com/pwoakk/restapidemo.git'
  }

  stage("Compilation") {
    sh "chmod +x ./gradlew"
    sh "./gradlew clean build -x test"
  }

  stage("Tests and Deployment") {
    stage("Running unit tests") {
      sh "chmod +x ./gradlew"
      sh "./gradlew test"
    }
    stage("Deployment") {
      sh "chmod +x ./gradlew"
      sh 'nohup ./gradlew bootRun -Dserver.port=8080 &'
      sh 'echo qweASD_123 | sudo docker login -u becka.kg99@gmail.com --password-stdin'
      sh 'sudo docker build -t pwoakk/restapidemo:1.0 .'
    }
    stage('Push Docker Image') {
        script {
            docker.withRegistry('https://index.docker.io/v1/', dockerHubCredentialsId) {
                def image = docker.image("pwoakk/restapidemo:1.0")
                image.push()
                image.push('1.0')
            }
        }
}
}
}