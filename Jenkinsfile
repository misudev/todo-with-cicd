pipeline {
    agent any
    options {
        timeout(time: 1, unit: 'HOURS')
    }
    environment {
        SOURCECODE_JENKINS_CREDENTIAL_ID = 'credentials'
        SOURCE_CODE_URL = 'https://github.com/inspirit941/todo-with-cicd.git'
        RELEASE_BRANCH = 'master'
    }
    stages {
        stage('Init') {
            steps {
                echo 'clear'
            }
        }

        stage('clone') {
            steps {
                git url: "$SOURCE_CODE_URL",
                    branch: "$RELEASE_BRANCH",
                    credentialsId: "$SOURCECODE_JENKINS_CREDENTIAL_ID"
                sh "ls -al"
            }
        }

        stage('frontend dockerizing') {
            steps {
                sh "docker build -t todo/frontend ./frontend"
            }
        }

        stage('backend dockerizing') {
            steps {
                sh "pwd"
                dir("./backend"){
                    sh "pwd"

                    sh "gradle clean"
                    sh "gradle bootJar"

                    sh "docker build -t todo/backend ."
                }
            }
        }

        stage('deploy') {
            steps {
                sh '''
                  docker run -d -p 5000:5000 todo/frontend

                  docker run -d -p 8080:8080 todo/backend
                '''
            }
        }
    }
}
