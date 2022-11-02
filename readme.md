# Spring PetClinic Pipeline with Jenkins & JFrog Artifactory
http://40.90.200.194:8080/job/artifactory/

We have configured this pipeline to fetch the source code from our code repo (https://github.com/dkhopade/spring-petclinic/blob/artifactory/) with branch ``artifactory``

## Understanding the Jenkins Pipeline
### Environment that let you connect to JFrog artifactory via ``ARTIFACTORY_ACCESS_TOKEN``
We are using Jfrog Artifactory to store our artifacts with this pipeline
```
environment {
    CI = true
    ARTIFACTORY_ACCESS_TOKEN = credentials('artifactory-access-token')
  }
```
### Stages
There are 4 main stages we will have with this pipeline. 
#### Stage 1: Compile. This stage will `compile` the source code using maven.
```
    stage('Compile') {
      steps {
        sh './mvnw compile'
      }
    }
```
#### Stage 2: Test. This stage will run the `test cases` from the source code using maven again.
```
    stage('Test') {
      steps {
        sh './mvnw test'
      }
    }
```
#### Stage 3: Build. This stage will `build` the source code using maven and store the JAR file to target/ folder.
```
    stage('Test') {
      steps {
        sh './mvnw clean install'
      }
    }
```
#### Stage 4: Upload JAR to Artifactory. This stage will `upload` the built JAR file from target/ folder and upload the binary JAR to JFrog Artifactory inside a repo called `spring-petclinic`.
```
    stage('Test') {
      steps {
        sh 'jf rt upload --url https://dkhopade.jfrog.io/artifactory --access-token ${ARTIFACTORY_ACCESS_TOKEN} target/spring-petclinic-2.6.0-SNAPSHOT.jar spring-petclinic/'
      }
    }
```

## Running petclinic locally with a docker container
To run the docker container from the stored image from artifactory; please follow the steps. Assuming you have Docker & Docker cli installed on your local machine, you should be able to run these steps.
### Pull container image
```
docker pull dkhopade.jfrog.io/docker-images/test-jfrog:latest
```
### Run container from the image
```
docker run -p 9090:8080 -d test-jfrog
```
### Finally access the app from your browser
```
http://localhost:9090/
```


## Building a Container if you wish to build a local image
There is a `Dockerfile` in this project. You can build a container image using docker cli:
```
docker build . -t test-jfrog:latest
```
