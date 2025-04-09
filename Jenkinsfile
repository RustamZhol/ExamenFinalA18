pipeline {
    agent any
    triggers {
        githubPush()
    }
    stages {
        stage('List Directory') {
            steps {
                script {
                    sh '''
                        if [ -d "/app" ]; then
                            echo "Contents of /app:"
                            ls -la /app
                        else
                            echo "/app not found. Contents of /:"
                            ls -la /
                        fi
                    '''
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("a18_rustamzhol_final_1277:latest", ".")
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(
                        credentialsId: 'dockerhub-password',
                        usernameVariable: 'DOCKER_HUB_USERNAME',
                        passwordVariable: 'DOCKER_HUB_TOKEN'
                    )]) {
                        sh "docker login -u ${env.DOCKER_HUB_USERNAME} -p ${env.DOCKER_HUB_TOKEN}"
                        sh "docker tag a18_rustamzhol_final_1277:latest ${env.DOCKER_HUB_USERNAME}/a18_rustamzhol_final_1277:latest"
                        sh "docker push ${env.DOCKER_HUB_USERNAME}/a18_rustamzhol_final_1277:latest"
                    }
                }
            }
        }
    }
}