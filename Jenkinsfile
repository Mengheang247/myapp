pipeline {
    agent any

    environment {
        // DOCKERHUB_CREDENTIALS = credentials('dockerhub')
        DOCKER_CREDENTIALS = "docker-hub-credentials"
        GIT_REPO_URL ="https://github.com/Mengheang247/myapp.git" 
    }


    stages {
        stage('Checkout') {
            steps {
                script {
                    checkout([$class: 'GitSCM',
                        branches: [[name: 'main']],
                        userRemoteConfigs: [[url: env.GIT_REPO_URL]]
                    ])
                }
            }
        }

        stage('Extract Version Tag') {
            steps {
                script {
                    // Get latest git tag (e.g., v1.0.0)
                    VERSION = sh(script: "git describe --tags --abbrev=0 || echo 'latest'", returnStdout: true).trim()
                    echo "🔖 Version Tag: ${VERSION}"
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // sh "docker build -t $IMAGE_NAME:$VERSION ."
                    sh "echo $VERSION"
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                  // Example build command
                      // withCredentials([usernamePassword(credentialsId: DOCKER_CREDENTIALS, usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                      //   sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                      // }
                  sh "echo Logging into Docker Hub..."
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    // sh """
                    // docker tag $IMAGE_NAME:$VERSION $IMAGE_NAME:latest
                    // docker push $IMAGE_NAME:$VERSION
                    // docker push $IMAGE_NAME:latest
                    // """
                    sh "echo Pushing version ${VERSION} to Docker Hub..."
                }
            }
        }

        stage('Deploy to Server') {
            steps {
                script{
                  echo "🚀 Deploying version ${VERSION} to server..."
                }
            }
        }
    }

    post {
        success {
            echo "✅ Deployment completed successfully for version ${VERSION}"
        }
        failure {
            echo "❌ Build or deploy failed!"
        }
    }
}