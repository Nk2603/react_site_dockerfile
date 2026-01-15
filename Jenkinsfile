pipeline {
 agent any
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', poll: false, url: 'https://github.com/Nk2603/react_site_dockerfile.git'
            }
        }

        stage('Build and Push Images') {
            steps {
                script {
                    sh 'docker build -t nk2603/food-react .'
                    withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'ay_pass', usernameVariable: 'ay_user')]) {
                        sh 'docker login -u $ay_user -p $ay_pass'
                        sh 'docker push nk2603/food-react'
                    }
                }
            }
        }

        stage('Deploy Services') {
            steps {
                script {
                    sh 'docker rm -f  react-app'
                    sh 'docker run -d --name react-app2 -p 2025:80 nk2603/food-react'
                }
            }
        }
        
        stage('Post Deployment Testing') {
            steps {
                script {
                    sh 'curl -I http://localhost:2025'
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check logs for details.'
        }
    }
}
