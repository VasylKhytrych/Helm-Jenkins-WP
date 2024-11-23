pipeline {
    agent any

    environment {
        ECR_REPOSITORY = '207567795251.dkr.ecr.eu-central-1.amazonaws.com/devops_task/main' // Replace with your ECR URL
        IMAGE_TAG = "v1.0.${BUILD_NUMBER}"
        DOCKER_IMAGE = "${ECR_REPOSITORY}:${IMAGE_TAG}"
    }


    stages {

        stage('Install Dependencies') {
            agent {
                docker {
                    image 'node:lts'
                }
            }
            steps {
                sh 'npm install'
            }
        }
        
        stage('Build Application') {
            steps {
                echo 'Installing Node.js dependencies...'
                sh 'npm install'
            }
        }

        stage('Run Unit Tests') {
            steps {
                echo 'Running Unit Tests...'
                sh 'npm test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo 'Running SonarQube Security Check...'
                withSonarQubeEnv('SonarQube') {
                    sh 'echo "SonarQube analysis simulated"'
                }
            }
        }

        stage('Build Docker Image') {
            when {
                expression {
                    return params.BUILD_DOCKER_IMAGE
                }
            }
            steps {
                echo 'Building Docker image...'
                sh """
                docker build -t ${DOCKER_IMAGE} .
                """
            }
        }

        stage('Push to ECR') {
            when {
                expression {
                    return params.BUILD_DOCKER_IMAGE
                }
            }
            steps {
                echo 'Pushing Docker image to ECR...'
                sh """
                aws ecr get-login-password --region eu-central-1 | docker login --username AWS --password-stdin ${ECR_REPOSITORY}
                docker push ${DOCKER_IMAGE}
                """
            }
        }

        stage('Deploy to Kubernetes') {
            when {
                expression {
                    return params.BUILD_DOCKER_IMAGE
                }
            }
            steps {
                echo 'Deploying to Kubernetes with Helm...'
                sh """
                helm upgrade --install simple-node-app ./helm-chart --set image.repository=${ECR_REPOSITORY} --set image.tag=${IMAGE_TAG}
                """
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '*.log', allowEmptyArchive: true
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}