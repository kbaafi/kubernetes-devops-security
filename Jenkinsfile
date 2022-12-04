pipeline {
  agent any

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar' //so that they can be downloaded later
            }
      }

      stage('Unit Tests') {
            steps {
              sh "mvn test"
            }
            post {
              always {
                junit 'target/surefire-reports/*.xml'
                jacoco execPattern: 'target/jacoco.exec'
              }
            }
      }

      stage ('Docker Build and Push') {
        steps {
          withDockerRegistry([credentialsId: "dockerhub", url: ""]){
            sh 'docker build -t kbaafi/numeric-app:""$GIT_COMMIT"" .'
            sh 'docker push kbaafi/numeric-app:""$GIT_COMMIT""'
          }
        }
      }
  }
}