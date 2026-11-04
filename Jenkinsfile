pipeline {
    agent {
        docker {
            image 'node:18-alpine'
            reuseNode true
        }
    }

    stages {
        stage('Build') {
            steps {
                sh '''
                    ls -al
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -al
                '''
            }
        }

        stage('Test') {
            steps {
                echo 'Test stage'

                sh 'test -f build/index.html'
                echo 'build/index.html 확인 완료'

                sh 'npm test -- --watchAll=false'
            }
        }
    }

    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}