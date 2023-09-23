pipeline {
    agent {
        label "jenkins-agent"
    }
    tools {
        nodejs 'node20.7.0' // Specify the Node.js tool installation name
    }
    environment {
        APP_NAME = "nodeapp"
        RELEASE = "1.0.0"
        DOCKER_USER = "test5532"
        DOCKER_PASS = 'dev@ops321'
        IMAGE_NAME = "${DOCKER_USER}/${APP_NAME}"
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
        JENKINS_API_TOKEN = credentials("JENKINS_API_TOKEN")
    }
    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }
        stage('Checkout Backend Code') {
            steps {
                // Checkout the backend code from the GitHub repository
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/Test5532/docker-app']]])
            }
        }
        stage('Build and Deploy Backend') {
            steps {
                dir('backend') {
                    // Navigate to the backend folder
                    // Run build and deployment steps specific to the backend application
                    sh 'npm install'
                    sh 'npm run build'
                    // Additional steps for deploying the backend
                }
            }
        }
        stage('Checkout Frontend Code') {
            steps {
                // Checkout the frontend code from the GitHub repository
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/Test5532/docker-app.git']]])
            }
        }
        stage('Build and Deploy Frontend') {
            steps {
                dir('frontend') {
                    // Navigate to the frontend folder
                    // Run build and deployment steps specific to the frontend application
                    sh 'npm install'
                    sh 'npm run build'
                    // Additional steps for deploying the frontend
                }
            }
        }
        stage("Build Docker Images") {
            steps {
                script {
                    // Build Docker images for both backend and frontend
                    def backendImage = docker.build("${IMAGE_NAME}-backend:${IMAGE_TAG}", "./backend")
                    def frontendImage = docker.build("${IMAGE_NAME}-frontend:${IMAGE_TAG}", "./frontend")

                    // Push the Docker images to a container registry
                    docker.withRegistry('', DOCKER_PASS) {
                        backendImage.push()
                        frontendImage.push()
                    }
                }
            }
        }
    }
    post {
        failure {
            emailext body: '''${SCRIPT, template="groovy-html.template"}''',
                    subject: "${env.JOB_NAME} - Build # ${env.BUILD_NUMBER} - Failed",
                    mimeType: 'text/html',
                    to: "karan.0requiem@gmail.com"
        }
        success {
            emailext body: '''${SCRIPT, template="groovy-html.template"}''',
                    subject: "${env.JOB_NAME} - Build # ${env.BUILD_NUMBER} - Successful",
                    mimeType: 'text/html',
                    to: "karan.0requiem@gmail.com"
        }
    }
}
