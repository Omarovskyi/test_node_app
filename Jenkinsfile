pipeline {
    agent { label 'jenkins-worker' }
    
    environment {
        GIT_URL = "https://github.com/Omarovskyi/test_node_app.git"
        GIT_BRANCH = "main"
        DOCKER_IMAGE_NAME = "omarovskyi/test_step_project_2"
        DOCKER_REGISTRY_URL = "https://registry.hub.docker.com"
        DOCKER_CREDENTIALS_ID = "omarovskyi"
    }

    stages {
      stage('Checkout') {
        steps {
          script {
            git url: "${GIT_URL}",
                branch: "${GIT_BRANCH}",
                credentialsId: "jenkins"
          }
        }
      }
      
      stage('Build') {
        steps {
          script {
            docker.build("${DOCKER_IMAGE_NAME}:${env.BUILD_NUMBER}")
          }
        }
      }
     
      stage('Build and run test') {
        steps {
          script {
            sh '''
                docker run --rm --name step_project_2 ${DOCKER_IMAGE_NAME}:${BUILD_NUMBER} test || { echo "Tests failed"; exit 1; }
            '''
          }
        }
      }

      stage('Push to Docker Hub') {
        steps {
          script {
            docker.withRegistry("${DOCKER_REGISTRY_URL}", "${DOCKER_CREDENTIALS_ID}") {
              docker.image("${DOCKER_IMAGE_NAME}:${env.BUILD_NUMBER}").push()
            }
          }
        }
      }
    }
}