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
         stage('docker build abd') {
            steps {
              withDockerRegistry([credentialsId:"docker-hub", url:""]) {
              sh 'printenv'
              sh 'docker build -t caloosha/numeric-apps:""$GIT_COMMIT"" .'
              sh 'docker push  caloosha/numeric-apps:""$GIT_COMMIT""'
            }
            }
        }
          stage('kubernetes deployment - DEV') {
            steps {
              withKubeConfig([credentialsId:"kubeconfig"]) {
              sh "sed -i 's#replace#caloosha/numeric-apps:${GIT_COMMIT}#g' k8s_deployment_service.yaml"
              sh "kubectl apply -f k8s_deployment_service.yaml"
            }
            }
        }
     
    }
}