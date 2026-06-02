pipeline {

    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials')
        DOCKERHUB_USERNAME    = 'assemragab105'   //
        IMAGE_NAME            = 'simple-java-app'
        IMAGE_TAG             = "latest"
    }

    stages {

        stage('Build App') {
            steps {
                echo '🔨 Building with Maven...'
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                echo '🧪 Running Tests...'
                sh 'mvn -f pom.xml test'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo '🐳 Building Docker Image...'
                sh """
                    docker build -t ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:${IMAGE_TAG} .
                    docker tag ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:${IMAGE_TAG} \
                               ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:latest
                """
            }
        }

        stage('Push to Docker Hub') {
            steps {
                echo '🚀 Pushing to Docker Hub...'
                sh """
                    echo ${DOCKERHUB_CREDENTIALS_PSW} | \
                    docker login -u ${DOCKERHUB_CREDENTIALS_USR} --password-stdin
                    docker push ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:${IMAGE_TAG}
                    docker push ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:latest
                """
            }
        }

    }

    post {
        success {
            echo '✅ Pipeline SUCCESS - Image pushed to Docker Hub!'
        }
        failure {
            echo '❌ Pipeline FAILED!'
        }
        always {
            sh """
                docker rmi ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:${IMAGE_TAG} || true
                docker rmi ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:latest || true
                docker logout || true
            """
        }
    }
}
