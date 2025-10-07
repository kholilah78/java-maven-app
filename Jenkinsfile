pipeline {
    agent {
        docker {
            image 'python:3.10'
             args '--user root'
        }
    }

    stages {
        stage('Install Dependencies') {
            steps {
                echo "Installing dependencies..."
                sh '''
                    pip install --user -r requirements.txt
                    export PATH=$PATH:/root/.local/bin
            } 
         }

        stage('Run Tests') {
            steps {
                echo "Running unit tests..."
                sh 'pytest test_app.py --maxfail=1 --disable-warnings -q'
            }
        }

        stage('Deploy') {
            when {
                anyOf {
                    branch 'main'
                    branch pattern: "release/.*", comparator: "REGEXP"
                }
            }
            steps {
                echo "Simulating deploy from branch ${env.BRANCH_NAME}"
                // Tambahkan deployment nyata di sini jika perlu:
                // sh 'docker build -t my-flask-app .'
                // sh 'docker run -d -p 5000:5000 my-flask-app'
            }
        }
    }

    post {
        success {
            script {
                def payload = [
                    content: "✅ Build SUCCESS on `${env.BRANCH_NAME}`\n🔗 URL: ${env.BUILD_URL}"
                ]
                httpRequest(
                    httpMode: 'POST',
                    contentType: 'APPLICATION_JSON',
                    requestBody: groovy.json.JsonOutput.toJson(payload),
                    url: 'https://discordapp.com/api/webhooks/1425102844259733686/rlemlapkYhB7BduudKfl06McK6ON_b1Z7MyDfT-upAl_4wG-S7bx8wy4_fd_YSvVK292'
                )
            }
        }

        failure {
            script {
                def payload = [
                    content: "❌ Build FAILED on `${env.BRANCH_NAME}`\n🔗 URL: ${env.BUILD_URL}"
                ]
                httpRequest(
                    httpMode: 'POST',
                    contentType: 'APPLICATION_JSON',
                    requestBody: groovy.json.JsonOutput.toJson(payload),
                    url: 'https://discordapp.com/api/webhooks/1425102844259733686/rlemlapkYhB7BduudKfl06McK6ON_b1Z7MyDfT-upAl_4wG-S7bx8wy4_fd_YSvVK292'
                )
            }
        }

        always {
            echo "Pipeline finished for branch ${env.BRANCH_NAME}"
        }
    }
}
