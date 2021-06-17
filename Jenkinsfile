pipeline {
    agent any
    environment {
        DOCKER_IMAGE_NAME = "nabil/train-kubernetes"
        DOCKER_IMAGE_TAG = 'latest'
        DOCKER_REGISTRY = 'r.deso.tech'
    }
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('Build Docker Image') {

            steps {
                script {
                    app = docker.build(DOCKER_IMAGE_NAME)
                    app.inside {
                        sh 'echo Hello, World!'
                    }
                }
            }
        }
        stage('Push Docker Image') {

            steps {
                script {
                    docker.withRegistry("https://${DOCKER_REGISTRY}", 'docker_hub_login') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }
        stage('DeployToProduction') {
   
        }
    }
}
