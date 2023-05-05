pipeline {
  agent any

  stages {
      stage('Build Artifact Abdi') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar' //so that they can be downloaded later
            }
        }
      stage('Test Artifact Abdi') {
            steps {
              sh "mvn test"
            }
            post {
              always{
                junit "target/surefire-reports/*.xml"
                jacoco execPattern: "target/jacoco.exec"
              }
            }
        }    
         stage('docker build ab') {
            steps {
              withDockerRegistry([credentialsId:"docker-hub", url:""]) {
              sh 'printenv'
              sh 'docker build -t caloosha/numeric-apps:""$GIT_COMMIT"" .'
              sh 'docker push -t caloosha/numeric-apps:""$GIT_COMMIT""'
            }
            }
        }
     
    }
}