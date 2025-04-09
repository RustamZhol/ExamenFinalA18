pipeline {
    agent any
    stages {
        stage('List Directory') {
            steps {
                script {
                    bat '''
                        IF EXIST "\\app" (
                            echo Contents of \\app:
                            dir \\app
                        ) ELSE (
                            echo \\app not found. Contents of \\:
                            dir \\
                        )
                    '''
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    bat 'docker build -t a18_rustamzhol_final_1277:latest .'
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(
                        credentialsId: 'docker-hub-credentials',
                        usernameVariable: 'DOCKER_HUB_USERNAME',
                        passwordVariable: 'DOCKER_HUB_TOKEN'
                    )]) {
                        bat 'docker login -u %DOCKER_HUB_USERNAME% -p %DOCKER_HUB_TOKEN%'
                        bat 'docker tag a18_rustamzhol_final_1277:latest %DOCKER_HUB_USERNAME%/a18_rustamzhol_final_1277:latest'
                        bat 'docker push %DOCKER_HUB_USERNAME%/a18_rustamzhol_final_1277:latest'
                    }
                }
            }
        }
    }
}