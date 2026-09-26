pipeline{
  agent{
    label "agent-01"
  }
  tools {
     jdk 'JDK-11'
     maven 'maven-3-5-4'
  }   

  stages{
    stage("Build Java App"){
        steps{
            sh "mvn package install -DskipTests=true"
        }
    }
    stage("Test Java App"){
        steps{
            sh "mvn test"
        }
    }
    stage("Build Docker Image"){
        steps{
            sh "docker build -t java-app:ver1 ."
        }
    }
  }
}