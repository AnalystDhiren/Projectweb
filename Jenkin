pipeline {
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/AnalystDhiren/Projectweb'
            }
        }
        stage('Code Analysis') {
            steps {
                script {
                    def scannerHome = tool 'SonarQube Scanner'
                    withSonarQubeEnv('SonarQube') {
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t my-app .'
            }
        }
        stage('Push Docker Image') {
            steps {
                withDockerRegistry([credentialsId: 'dockerhub-credentials', url: '']) {
                    sh 'docker tag my-app:latest your-dockerhub/my-app:latest'
                    sh 'docker push your-dockerhub/my-app:latest'
                }
            }
        }
        stage('Deploy Application') {
            steps {
                sh 'docker run -d -p 80:80 your-dockerhub/my-app:latest'
            }
        }
    }
}
