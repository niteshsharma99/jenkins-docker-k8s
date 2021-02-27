pipeline {
  agent any
  stages {
    stage('Build-Image') {
      steps {
        sh 'docker build -t nitesh99sharma/jenkins-docker-kubectl":$BUILD_NUMBER" .'
      }
    }
      stage ('Push-Image') {
        steps {
        withDockerRegistry([ credentialsId: "dockerhub_id", url: "" ]) {
          sh 'docker push nitesh99sharma/jenkins-docker-kubectl":$BUILD_NUMBER"'

        }
      }
    }
  }
}
