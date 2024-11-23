pipeline {
    agent any

    parameters {
        booleanParam(name: 'BUILD_DOCKER_IMAGE', defaultValue: true, description: 'Build and push Docker image')
        string(name: 'IMAGE_TAG', defaultValue: 'v1.0', description: 'Tag for Docker image')
    }

    environment {
        ECR_REPOSITORY = '207567795251.dkr.ecr.eu-central-1.amazonaws.com/devops_task/main'
        IMAGE_TAG = "${params.IMAGE_TAG}.${BUILD_NUMBER}"
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

        stage('Build Docker Image') {
            when { expression { return params.BUILD_DOCKER_IMAGE } }
            agent {
                kubernetes {
                    yamlFile 'path-to-docker-enabled-agent.yaml'
                }
            }
            steps {
                sh "docker build -t ${DOCKER_IMAGE} ."
            }
        }

        stage('Push to ECR') {
            when { expression { return params.BUILD_DOCKER_IMAGE } }
            steps {
                withCredentials([usernamePassword(credentialsId: 'ecr-credentials', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                    sh """
                    aws ecr get-login-password --region eu-central-1 | docker login --username AWS --password-stdin ${ECR_REPOSITORY}
                    docker push ${DOCKER_IMAGE}
                    """
                }
            }
        }

        stage('Deploy to Kubernetes') {
            when { expression { return params.BUILD_DOCKER_IMAGE } }
            steps {
                withCredentials([file(credentialsId: 'kubeconfig', variable: 'KUBECONFIG')]) {
                    sh """
                    helm upgrade --install simple-node-app ./helm-chart \
                      --set image.repository=${ECR_REPOSITORY} \
                      --set image.tag=${IMAGE_TAG}
                    """
                }
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
