@Library('jenkins-sharedlib') _

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
                script{
                    def mvn = new edu.depi.maven()
                    mvn.mavenCommand("package install -DskipTests=true")
                }
            }
        }

        stage("Test Java App") {
            agent {
        label "agent-01"
    }
            steps {
                script{
                    def test = new edu.depi.maven()
                    test.mavenCommand("test")
                }
            }
        }

        stage("Build Docker Image") {
            agent {
        label "agent-01"
    }
            steps {
                script{
                    def build = new edu.depi.docker()
                    build.dockerBuild("fadel8/repo_1", "${BUILD_NUMBER}")
                }
            }
        }

        stage("Login to Docker Hub") {
            agent {
        label "agent-01"
    }
            steps {
                script{
                    def login = new edu.depi.docker()
                    login.dockerLogin("${username}", "${password}")
                }
            }
        }

        stage("Push Docker Image") {
            agent {
        label "agent-01"
    }
            steps {
                script{
                    def push = new edu.depi.docker()
                    push.dockerPush("fadel8/repo_1", "${BUILD_NUMBER}")
                }
            }
        }
        stage("Deploy Docker image"){
            agent{
                label "agent-02"
            }
            steps{
                sh'''
                Check_Image=$(docker ps -a | grep java-app) || true
                if [[ -n $Check_Image ]]; then docker rm -f java-app && docker run -d -p 8090:8090 --name java-app fadel8/repo_1:v${BUILD_NUMBER}; else docker run -d -p 8090:8090 --name java-app fadel8/repo_1:v${BUILD_NUMBER}; fi
                '''
            }
        }
    }
}