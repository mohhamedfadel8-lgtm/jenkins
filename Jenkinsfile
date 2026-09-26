pipeline{
  agent{
    label "agent-01"
  }
  tools {
     jdk 'JDK-11'
     maven 'maven-3-5-4'
  }
  environment {
  Username = credentials("Docker-username")
  Password = credentials("Docker_Password")
}
  stages{
    stage("Check SCM"){
        steps{
            git branch: 'main', url: 'https://github.com/mohhamedfadel8-lgtm/jenkins.git'
        }
    }
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
            sh "docker build -t fadel8/repo_1:${BUILD_NUMBER} ."
        }
    }
    stage("Login to docker hub"){
        steps{
            sh "docker login -u ${Username} -p ${Password}"
        }
    }
    stage("Push Docker Image"){
        steps{
            sh "docker push fadel8/repo_1:v${BUILD_NUMBER}"
        }
    }
  }
}