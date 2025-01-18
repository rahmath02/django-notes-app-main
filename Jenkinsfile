pipeline {
    agent any

    environment {
        IMAGE_NAME = "notes-app-jenkins"
    }

    stages {
        stage("Clone Code") {
            steps {
                git branch: "main", url: "https://github.com/rahmath02/django-notes-app-main.git"
                echo "Aaj toh LinkedIn Post bannta hai boss"
            }
        }

        stage("Build & Test") {
            steps {
                sh "docker build . -t notes-app-jenkins:latest"
            }
        }

        stage("Push to DockerHub") {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: "dockerCreds",
                        passwordVariable: "dockerHubPass",
                        usernameVariable: "dockerHubUser"
                    )
                ]) {
                    script {
                        sh "docker image tag ${IMAGE_NAME}:latest ${dockerHubUser}/${IMAGE_NAME}:latest"
                        sh "echo ${dockerHubPass} | docker login -u ${dockerHubUser} --password-stdin"
                        sh "docker push ${dockerHubUser}/${IMAGE_NAME}:latest"
                    }
                }
            }
        }

        stage("Deploy") {
            steps {
                sh "docker rm -f notes-app"
                sh "docker run -d -p 8000:8000 --name notes-app notes-app-jenkins:latest"
            }
        }
    }
}
