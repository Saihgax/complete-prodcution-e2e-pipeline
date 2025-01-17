pipeline {
    agent{
        label "jenkins-agent"
    }
    tools{
        jdk "Java17"
        maven "Maven3"
    }
    environment {
        APP_NAME = "complete-prodcution-e2e-pipeline"
        RELEASE = "1.0.0"
        DOCKER_USER = "saihgax"
        DOCKER_PASS = 'dockerhub'
        IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
        // JENKINS_API_TOKEN = credentials("JENKINS_API_TOKEN")

    }
    stages {
        stage("Cleanup workspace") {

            steps {
                cleanWs()
            }
        }
        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/Saihgax/complete-prodcution-e2e-pipeline'
            }
        }
        stage("Build Application") {
            steps {
                sh "mvn clean package"
            }
        }
        stage("Test Application") {
            steps {
                sh "mvn test"
            }
        }
        stage("Build & Push Docker Image") {
            steps {
                script{
                    docker.withRegistry('', DOCKER_PASS){
                        docker_image = docker.build "${IMAGE_NAME}:${IMAGE_TAG}"
                    }

                    docker.withRegistry('', DOCKER_PASS) {
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push('latest')
                    }
                }
            }
        }
    }
}
