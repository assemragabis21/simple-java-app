pipeline {
    agent any

    environment {
        APP_NAME = 'jenkins-practice'
        BUILD_ENV = 'staging'
    }

    stages {
        stage('Checkout Info') {
            steps {
                echo "App: ${APP_NAME}"
                echo "Env: ${BUILD_ENV}"
                echo "Branch: ${GIT_BRANCH}"
                echo "Commit: ${GIT_COMMIT}"
            }
        }

        stage('Simulate Build') {
            steps {
                sh 'echo compiling code...'
                sh 'mkdir -p build && echo artifact > build/app.jar'
                sh 'ls -la build/'
            }
        }

        stage('Simulate Tests') {
            parallel {
                stage('Unit') {
                    steps { sh 'echo running unit tests && sleep 2' }
                }
                stage('Integration') {
                    steps { sh 'echo running integration tests && sleep 2' }
                }
            }
        }

        stage('Simulate Deploy') {
            steps {
                echo "Deploying ${APP_NAME} build #${BUILD_NUMBER} to ${BUILD_ENV}"
                sh 'echo deployed!'
            }
        }
    }

    post {
        always {
            echo "Build #${BUILD_NUMBER} finished — ${currentBuild.currentResult}"
        }
    }
}
