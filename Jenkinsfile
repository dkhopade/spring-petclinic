pipeline {
  agent { label 'Built-In-Node' }
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    CI = true
    ARTIFACTORY_ACCESS_TOKEN = credentials('artifactory-access-token')
  }
  stages {
    stage('Compile') {
      steps {
        sh './mvnw compile'
      }
    }
    stage('Test') {
      steps {
        sh './mvnw test'
      }
    }
    stage('Build') {
      steps {
        sh './mvnw clean install'
      }
    }
    stage('Upload JAR to Artifactory') {
      steps {
        sh 'jf rt upload --url https://dkhopade.jfrog.io/artifactory --access-token ${ARTIFACTORY_ACCESS_TOKEN} target/spring-petclinic-2.6.0-SNAPSHOT.jar spring-petclinic/'
      }
    }
    stage('Upload Docker Container Image to Artifactory') {
      steps {
        sh 'docker login -udeepak.khopade@gmail.com dkhopade.jfrog.io -p${ARTIFACTORY_ACCESS_TOKEN}'
        sh 'docker build . -t test-jfrog:latest'
        sh 'docker tag test-jfrog:latest dkhopade.jfrog.io/docker-images/test-jfrog:latest'
        sh 'docker push dkhopade.jfrog.io/docker-images/test-jfrog:latest'
      }
    }
  }
}
