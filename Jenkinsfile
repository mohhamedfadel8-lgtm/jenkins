pipeline {

    agent none

    tools {
        jdk 'JDK-11'
        maven 'maven-3-5-4'
    }

    environment {
        Username = credentials("Docker-username")
        Password = credentials("Docker_Password")
    }

    stages {

        stage("Check SCM") {
            agent {
        label "agent-01"
    }
            steps {
                git branch: 'main',
                    url: 'https://github.com/mohhamedfadel8-lgtm/jenkins.git'
            }
        }

        stage("Build Java App") {
            agent {
        label "agent-01"
    }
            steps {
                sh 'mvn package install -DskipTests=true'
            }
        }

        stage("Test Java App") {
            agent {
        label "agent-01"
    }
            steps {
                sh 'mvn test'
            }
        }

        stage("Build Docker Image") {
            agent {
        label "agent-01"
    }
            steps {
                sh 'docker build -t fadel8/repo_1:${BUILD_NUMBER} .'
            }
        }

        stage("Login to Docker Hub") {
            agent {
        label "agent-01"
    }
            steps {
                sh 'echo "$Password" | docker login -u "$Username" --password-stdin'
            }
        }

        stage("Push Docker Image") {
            agent {
        label "agent-01"
    }
            steps {
                sh 'docker push fadel8/repo_1:${BUILD_NUMBER}'
            }
        }
        stage("Deploy Docker image"){
            agent{
                label "agent-02"
            }
            steps{
                sh'docker run -d -p 8090:8090 --name java-app fadel8/repo_1:v${BUILD_NUMBER}'
            }
        }
    }
}