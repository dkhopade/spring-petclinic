pipeline {
  agent { label 'Built-In Node' }
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    CI = true
    ARTIFACTORY_ACCESS_TOKEN = credentials('artifactory-access-token')
  }
  stages {
    stage('Test') {
      steps {
        sh './mvnw clean install'
      }
    }
    stage('Build') {
      steps {
        sh './mvnw clean install'
      }
    }
    stage('Upload to Artifactory') {
      agent {
        docker {
          image 'releases-docker.jfrog.io/jfrog/jfrog-cli-v2:2.2.0'
          reuseNode true
        }
      }
      steps {
        sh 'jf rt upload --url https://dkhopade.jfrog.io/ --access-token ${ARTIFACTORY_ACCESS_TOKEN} target/spring-petclinic-2.6.0-SNAPSHOT.jar spring-petclinic/'
      }
    }
  }
}
