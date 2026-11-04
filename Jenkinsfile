pipeline {
    agent {
        docker {
            image 'node:18-alpine'
        }
    }

    stages {
        stage('Build') {
            steps {
                sh '''
                    npm ci
                    npm run build
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
}