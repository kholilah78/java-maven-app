pipeline {
    agent any

    tools {
        maven 'maven-app'
    }

    stages {
        stage('Build jar') {
            steps {
                sh 'mvn package'
            }
        }

        stage('Build Image') {
            steps {
                script {
                    echo "Building the Docker image..."
                    withCredentials([usernamePassword(credentialsId: '03548c8a-56f7-4f80-980d-ebe0c01b8945', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        sh 'docker build -t kholilah/demo-app:jmp-2.0 .'
                        sh "echo \$PASS | docker login -u \$USER --password-stdin"
                        sh 'docker push kholilah/demo-app:jmp-2.0'
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo "Deploying the application..."
                }
            }
        }
    }
}
