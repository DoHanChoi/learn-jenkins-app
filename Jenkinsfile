pipeline {
    agent none

    environment {
        REACT_APP_VERSION = "1.0.$BUILD_ID"
        APP_NAME = 'myjenkinsapp'
        AWS_DEFAULT_REGION = 'ap-northeast-2'
        AWS_DOCKER_REGISTRY = '614388036068.dkr.ecr.ap-northeast-2.amazonaws.com'
        AWS_ECS_CLUSTER = 'hollow-rabbit-1mib6o'
        AWS_ECS_SERVICE_PROD = 'LearnJenkinsApp-Service-Prod'
        AWS_ECS_TD_PROD = 'LearnJenkinsApp-TaskDefinition-Prod3'
    }


    stages {

        

        


        stage('Build') {

            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.39.0-jammy' 
                    reuseNode true
                }
            }
            

            steps {
                sh '''
                    echo '트리거 테스트 중 ...'
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }

        stage('Test') {
            steps {
                echo 'Test stage'
            }
        }

        
    }
}
